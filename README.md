# 1. 웹 소켓을 이용한 업비트 데이터 긁어오기.

웹 소켓을 이용해 업비트의 실시간 시세, 거래량, 최근 5일간의 종가를 바탕으로 상승장인지 하락장인지 구분하여 화면에 출력합니다.
(웹 소켓에 최근 5일 데이터를 바탕으로 상승장 하락장을 구분하는 함수를 추가하여 속도가 많이 느려졌습니다.)
더 빠른 정보를 원하시면 상승/하락 장을 구분하는 메소드는 제거하셔도 좋습니다.

example.ui 는 아나콘다 파일입니다.

# 2. 1번 파일을 이용한 자동매매 알고리즘.
기존의 파일은 한개의 종목만 웹 소켓으로 가져와 자동 매수 진행
-> 여러개의 종목에 알고리즘 적용 -> 속도가 느려져 수정중에 있습니다.
