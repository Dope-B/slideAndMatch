# slideAndMatch

#### 시용기술: visualMicro
#### 시용 부품
- 1.8인치 터치패드

#### 설명
- 터치패드는 상단, 하단으로 나뉜다.
  - 상단에는 주기적으로 상하좌우로 랜덤 색깔의 오브젝트들이 생성된고 가운데 고정 오브젝트가 있다.
  - 하단에는 특정 방향으로 슬라이드 해서 가운데 오브젝트의 색을 바꾼다.
- 이동 오브젝트가 가운데 오브젝트와 겹치기 전에 색깔을 맞춘다.
- 이동 오브젝트의 색, 속도는 랜덤이다. 
- 이동 오브젝트는 화면에 최대 8개가 출력된다(오브젝트 풀링)
- 전원 인가 시 터치하여 시작한다.
```C++
if (millis() - start_term > 500) {// 택스트가 1초를 주기로 깜빡임
		start_term = millis();
		if (start_print) { tft.setCursor(40,200); tft.print("Touch to play"); start_print = false; }
		else { tft.fillRect(40,200, 160, 20, WHITE); start_print = true; }
		}
```
- 1초에 5프레임씩 움직인다.
- 점수가 높아지면 이동 오브젝트 스폰 텀이 짧아진다.
- 이동 오브젝트 생성은 다음과 같다.
```C++
void arrowenable(int i) {
	Acolor[i] = random(4);// 색 결정
	switch (Acolor[i])
	{
	case 0:
		Acolor[i] = RED;
		break;
	case 1:
		Acolor[i] = BLUE;
		break;
	case 2:
		Acolor[i] = GREEN;
		break;
	default:
		Acolor[i] = YELLOW;
		break;
	}
	Adirection[i] = random(4);// 0-> left 1->right 2->up 3->down// 방향 결정
	switch (Adirection[i])
	{
	case 0:
		Aloc_x[i] = 240;
		Aloc_y[i] = 85;
		break;
	case 1:
		Aloc_x[i] = 0;
		Aloc_y[i] = 85;
		break;
	case 2:
		Aloc_x[i] = tri_x;
		Aloc_y[i] = 150;
		break;
	default:
		Aloc_x[i] = tri_x;
		Aloc_y[i] = 0;
		break;
	}
	if (score < 1000) {
		Aspeed[i] = random(2, 4);// 속도 결정
	}
	else {
		Aspeed[i] = random(2, 10);
	}
}
```
- 터치 감지는 다음 조건을 만족해야 한다.
  - 하단 패널이 색 변경 중이 아니여야함
  - 터치 압력이 일정 수준 이상
  - 터치 점의 x좌표가 5 이상
- 슬라이드 시 하단 패널의 색 변경은 슬라이드 방향으로 한줄 씩 이뤄진다.(current_color_change())
- 슬라이드 판정은 다음과 같다.
  - 최초 터치점과 현재 터치점의 x좌표 거리가 25이상, y좌표 거리가 15이상이여야 한다.
  - x좌표 거리가 y좌표 거리의 2.5 이상이면 오른쪽 슬라이딩으로 판정하는 등<br/> 각 축의 변화량에 따라 슬라이딩 방향을 결정한다.
- 8번 실패하면 게임이 리셋된다.

#### 피드백
- 아직 터치 시 글리치가 있다.
- 게임 플레이 시간이 길어질 시 프레임이 떨어진다.
