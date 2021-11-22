# caerang_sw_2021


import sys
from PyQt5.QtWidgets import *
from PyQt5 import uic
from PyQt5.QtCore import *
import pyupbit
import time

tickers = pyupbit.get_tickers(fiat="KRW")
cnt = len(tickers)
form_class = uic.loadUiType("example.ui")[0]

class Worker(QThread):
    finished = pyqtSignal(dict)

    def run(self):
        while True:
            data = {}

            for ticker in tickers:
                data[ticker] = self.get_market_infos(ticker)

            self.finished.emit(data)
            time.sleep(2)

    def get_market_infos(self, ticker):
        try:
            df = pyupbit.get_ohlcv(ticker)
            ma5 = df['close'].rolling(window=5).mean()

            price = pyupbit.get_current_price(ticker)
            last_ma5 = ma5[-2]

            state = None
            if price > last_ma5:
                state = "상승장"
            else:
                state = "하락장"

            return (price, last_ma5, state)
        except:
            return (None, None, None)


class MyWindow(QMainWindow, form_class):
    def __init__(self):
        super().__init__()
        self.setupUi(self)

        # self.tableWidget.setRowCount(len(tickers))
        self.worker = Worker()
        self.worker.finished.connect(self.update_table_widget)
        self.worker.start()

    @pyqtSlot(dict)

    def update_table_widget(self, data):
        try:
            for ticker, infos in data.items():
                index = tickers.index(ticker)
                print(data.items())
                self.tableWidget.setItem(index, 0, QTableWidgetItem(ticker))
                self.tableWidget.setItem(index, 1, QTableWidgetItem(str(infos[0])))
                self.tableWidget.setItem(index, 2, QTableWidgetItem(str(infos[1])))
                self.tableWidget.setItem(index, 3, QTableWidgetItem(str(infos[2])))
        except:
            pass


app = QApplication(sys.argv)
win = MyWindow()
win.show()
app.exec_()
