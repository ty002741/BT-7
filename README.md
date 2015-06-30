# RPi實做BT-711111111111

##題目發想緣起
身為熱血男兒，心中總對坦克車這種曾經稱霸陸地的軍是載具有著一種莫名的愛慕。因此，在那份熱血的驅使之下，決定利用這次專題的機會，開始打造自己的坦克。<br>

BT-7，是蘇聯1935至1940年間生產的一種輕型坦克，其最大特色是將履帶卸除後一就可以使用路輪行駛。會選擇他當作本次專案的題目其實是因為當初決定要製作坦克之後，想使用Moli實驗室的3D列印打造車體，但是當時列印的材料已經不夠了，因此本來會改成製作四輪驅動車，就很像是坦克車拿掉履帶後剩下路輪的緣故。<br>

##實作所需材料（取得來源、價位）

<table>
	<thead>
		<tr>
			<td>材料名稱</td>
			<td>數量</td>
			<td>單價</td>
			<td>總價</td>
			<td>來源</td>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>杜邦線(公母)</td>
			<td>總數40條(使用6條)</td>
			<td>NTD90</td>
			<td>NTD90</td>
			<td rowspan="3">電子材料行</td>
		</tr>
		<tr>
			<td>杜邦線(母母)</td>
			<td>總數40條(使用4條)</td>
			<td>NTD100</td>
			<td>NTD100</td>
		</tr>
		<tr>
			<td>電池盒(3號8入)</td>
			<td>1個</td>
			<td></td>
			<td></td>
		</tr>
		<tr>
			<td>雙層四驅自走車組(含4馬達)</td>
			<td>一組(使用兩顆馬達)</td>
			<td>NTD650</td>
			<td>NTD650</td>
			<td rowspan="2"><a href="http://www.raspberrypi.com.tw/purchase/shop/robotics/">台灣樹梅派網站</a></td>
		</tr>
		<tr>
			<td>L298N馬達控制板</td>
			<td>1個</td>
			<td>NTD160</td>
			<td>NTD160</td>
		</tr>
		<tr>
			<td>鐵絲</td>
			<td>6米</td>
			<td>NTD20</td>
			<td>NTD20</td>
			<td rowspan="4">五金行</td>
		</tr>
		<tr>
			<td>1分螺絲1.5吋(10入)</td>
			<td>總數10個(使用2個)</td>
			<td>NTD6</td>
			<td>NTD6</td>
		</tr>
		<tr>
			<td>1分螺絲1吋(10入)</td>
			<td>總數10個(使用2個)</td>
			<td>NTD6</td>
			<td>NTD6</td>
		</tr>
		<tr>
			<td>1分螺絲冒(10入)</td>
			<td>總數10個(使用6個)</td>
			<td>NTD6</td>
			<td>NTD6</td>
		</tr>
		<tr>
			<td>3D列印_車體_block</td>
			<td>2個</td>
			<td>5g</td>
			<td>10g</td>
			<td rowspan="5"><a href="https://www.facebook.com/MOLi.rocks">MOLi</a>友情贊助</td>
		</tr>
		<tr>
			<td>3D列印_車體_chassis</td>
			<td>1個</td>
			<td>30g</td>
			<td>30g</td>
		</tr>
		<tr>
			<td>3D列印_車體_idler_wheel</td>
			<td>2個</td>
			<td>11.8g</td>
			<td>23.6g</td>
		</tr>
		<tr>
			<td>3D列印_車體_motor_wheel</td>
			<td>2個</td>
			<td>11.8g</td>
			<td>23.6g</td>
		</tr>
			<td>3D列印_車體_tread</td>
			<td>48個</td>
			<td>3g</td>
			<td>144g</td>
		</tr>
	</tbody>
</table>
##使用的現有軟體與來源
- PuTT：[PuTTY官網](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
- UP! V2.1：[UP3D官網](http://www.pp3dp.com/)
- [python-RPi.GPIO](http://rsapberrypi.collabora.com)

##實作過程（碰到哪些問題、如何解決）
一開始再設定Raspberrt Pi 2時，因為手上剛好有多餘的鍵盤滑鼠與HDMI線，因此打算直接輸出螢幕來進行後續的實做，在設定HDMI輸出的參數時，有蠻多項目可以設定，因此花了一些時間在這個項目的設定上。<br>

控制的程式大致上都寫好之後，必須開始處理有關搖控的部分，由於手上有的設備只有Wifi網卡，因此打算透過WiFi來連線，但是我們安裝的raspbian版本在圖形化介面中並沒有看到直接設定WiFi的，於是回到command line的模式下直接對無線網路的設定檔做修改，但是原先對Linux的設定設定文件作編輯並沒有太多經驗，又遇上很多網路上的教學
文件版本不大一樣，因此只能土法煉鋼的一樣一樣嘗試。<br>

當RPi成功連上網路後，下一步就是要準備解決操作的問題，一開始想採取SSH直接連線的模式來操作，但是我們使用的python操作介面是需要使用GUI的介面，因此在SSH連線的模式下並無法正常運作，但是短時間內我們也無法把整個操作介面更改掉，因此最後是採用RDP遠端桌面協議的方式來解決，雖然不是完美的遙控解決方案，但不失為一個暫時性的低成本接決方案。<br>
當我們在組裝車身的時候，我們發現上面的孔徑大小不對，所以我們拿了電鑽開始將洞弄成我們需要的孔徑，結果事情不如預期的順利，一開始我們上方電鑽往下鑽，不料塑膠材質韌性不足，所以斷裂，而事發當日剛好又是評審的前一天，所以我們做了危機處理，以膠帶和束帶先行固定。效果還不錯如下圖。<br>

![GitHub Logo](https://github.com/NCNU-OpenSource/BT-7/blob/master/img/001.jpg)<br>

##運用哪些與課程內容中GPIO線材相關的技巧
使用USB TTL線才連接Raspberry Pi 2 設定相關參數。<br>

##組裝過程及製作教學（GPIO線材的安裝、3D列印後的組裝...）

###3D列印<br>
首先請到pp3dp 下載[3DUP](http://www.pp3dp.com/)設備列印程式，再到[thingiverse](http://www.thingiverse.com/) 這個網站可以下載 3D模組，而我們下載的是[Mini tank](http://www.thingiverse.com/thing:132199) 模組裡面有五個模組，分別是tread.stl、block.stl、chassis.stl、motor_wheel.stl、idler_wheel.stl。<br>

![GitHub Logo](https://github.com/NCNU-OpenSource/BT-7/blob/master/img/002.png)<br>
開啟：開啟所下在地stl檔，將畫好的3D模組載入控制台中(可以同時多個模組)。<br>
存檔：可以將擺放好的模組樣式分法儲存成UPP檔案。<br>
卸載：可以將用不到的模組從控制台種卸載。<br>
![GitHub Logo](https://github.com/NCNU-OpenSource/BT-7/blob/master/img/003.png)<br>
最佳視圖：當不小心將圖層拉太遠，或是拉不回來的時候，按下去就會回到正前方最佳觀賞之位子。<br>
![GitHub Logo](https://github.com/NCNU-OpenSource/BT-7/blob/master/img/004.png)<br>
移動：選取要移動的物件，可以透過XYZ軸移動物件，也可以按著ctrl加滑鼠直接拖曳。<br>
旋轉：選取要旋轉的物件，可以透過XYZ設定角度旋轉物件。<br>
縮放：等比例縮放3D模組。<br>
![GitHub Logo](https://github.com/NCNU-OpenSource/BT-7/blob/master/img/005.png)<br>
此圖為3D列印擺放是意圖。

###組裝
準備好實作所需之材料後
tread以24個為一個履帶組以下面的方式組合。
![GitHub Logo](https://github.com/NCNU-OpenSource/BT-7/blob/master/img/006.jpg)<br>
![GitHub Logo](https://github.com/NCNU-OpenSource/BT-7/blob/master/img/007.jpg)<br>
![GitHub Logo](https://github.com/NCNU-OpenSource/BT-7/blob/master/img/008.jpg)<br>
![GitHub Logo](https://github.com/NCNU-OpenSource/BT-7/blob/master/img/009.jpg)<br>

idler_wheel 與 block 各一個組合，如下方組合方式組合
![GitHub Logo](https://github.com/NCNU-OpenSource/BT-7/blob/master/img/010.jpg)<br>
![GitHub Logo](https://github.com/NCNU-OpenSource/BT-7/blob/master/img/011.jpg)<br>
![GitHub Logo](https://github.com/NCNU-OpenSource/BT-7/blob/master/img/012.jpg)<br>
motor_wheel 、 block和馬達 各一個組合。
![GitHub Logo](https://github.com/NCNU-OpenSource/BT-7/blob/master/img/013.jpg)<br>
再將以上的東西組合到chassis上。
![GitHub Logo](https://github.com/NCNU-OpenSource/BT-7/blob/master/img/014.jpg)<br>
再將履帶裝上。
![GitHub Logo](https://github.com/NCNU-OpenSource/BT-7/blob/master/img/015.jpg)<br>

###GPIO線材的安裝
![GitHub Logo](https://github.com/NCNU-OpenSource/BT-7/blob/master/img/016.png)<br>

###程式
本次專案的程式碼主要著重在GPIO的輸出控製。<br>
首先要設定我們程式終如何定義GPIO的腳位，在python-RPi.gpio中有兩種呼叫方式，分別為<br>
gpio.setmode(gpio.BOARD)<br>

或<br>

gpio.setmode(gpio.BCM)<br>
這裡我們選擇gpio.setmode(gpio.BOARD)來使用，BOARD的呼叫方式是使用右圖的中間兩排數字來區別各pin腳，因此在操作上個人覺得比較方便，不需要一直對照圖表，而且也能與硬體接線上的數字相同，方便整個專案中的溝通。<br>
![GitHub Logo](https://github.com/NCNU-OpenSource/BT-7/blob/master/img/017.png)<br>
接下來則是設定ping腳的輸出或輸入模式，這裡我們選擇使用gpio.setup(pin, gpio.OUT)，使用gpio.OUT模式時，該pin腳只會有輸出與不輸出兩種狀態，對我們的操控已經是夠用了。<br>
接下來就是實作控制的部分了<br>
gpio.output(pin, True/False)<br>
此行程式碼主要控制該ping腳的輸出狀態，已本次專案為例13與15腳為接到L298N的IN2與IN1，因此13腳位True、15腳位False時，L298N的OUT2送電(正極)、OUT1不送電(負極)，此時馬達為正轉可控制右側履帶前進，以此類推可以組合出各式操控的組合。<br>
##操作教學（做出此產品之後該如何操作）
連接上之後開啟robotics目錄下的robot3.py，進到操作介面後，使用鍵盤控製。

w：前進<br>
s：後退<br>
a：左轉<br>
d：右轉<br>
z：倒左轉<br>
c：倒右轉<br>
q：原地左迴轉<br>
e：原地右迴轉<br>

##工作分配表
構想：黃伯雄、游晁偉、黃麟翔<br>
材料準備：黃麟翔、游晁偉<br>
車身組裝：黃麟翔、黃伯雄<br>
程式：黃伯雄、游晁偉<br>

##參考來源
Mini Tank by samp20<br>
http://www.thingiverse.com/thing:132199/#instructions <br>
Rbotics and the Raspberry Pi By sentdex<br>
https://www.youtube.com/playlist?list=PLQVvvaa0QuDeJlgD1RX9_49tMLUxvIxF4<br>
Raspberry Pi超炫專案與完全實戰By柯博文 ， 碁峰<br>
ISBN： 9789863474739<br>
