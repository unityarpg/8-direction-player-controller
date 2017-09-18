# Unity 系統展示 - 8方向主角控制系統
這個系統展示主要是要讓玩家在 Unity 裡能夠操控主角往 8 種方向進行移動，你可以用WASD或方向鍵來操控主角並且可往對角方向移動。

在開發這個控制系統展示之前，我有稍微看了一下其他人所寫的角色操控方式，其思路不外乎就是判斷KeyCode然後決定transform的向量並移動人物。

例如按下W就往前移動多少，按下D就往右移動多少，所以4個按鍵總共要寫4次判斷。

這個方法沒有不好，只是很麻煩，不僅在面向的操控上不太方便，而每一個方向都要判斷一次，有的不會寫switch就會看到畫面上滿滿的if...

有沒有什麼方法可以讓8種方向的按鍵組合都通用，還不需要用一堆if判斷按鍵就能完美地移動人物，如此一來就不用每次開新專案都要為了一個基本的移動系統而寫一長串的程式碼？

# 原理介紹
玩家透過按鍵操控人物往哪一個方向走時，遊戲人物是利用向量來決定行走距離以及方向的。

例如說，按下W代表要往上走，而用向量來表示的話就是(0,1)，D就是(1,0)，依此類推。

同理，當我們要操控人物往對角線方向走時，其按鍵剛好就是兩個向量的合成，例如按下WD代表要往右上走，兩個向量相加會變成(1,1)。

根據這個思路，我們可以把按鍵製作成一組向量並讓人物根據這個向量來決定正面要朝哪並往正面移動，如此一來我們就不用逐一判斷所有按鍵，只要將按鍵組合成向量後就能讓人物根據這個向量進行移動了。

在按鍵偵測上，本系統使用Input.GetAxisRaw來取得按鍵值，Input.GetAxisRaw("Horizontal")會取得-1/0/1三種值，分別為按下A/沒按/按下D。

而Input.GetAxisRaw("Vertical")當然就是W、S了，在獲得按鍵值後，我們就能按照下列方式組成一個3維向量：

Vector3 PlayerDirection = new Vector3(Input.GetAxisRaw("Horizontal"), 0, Input.GetAxisRaw("Vertical"));

這樣就有了移動向量，再來就只要命令人物往這個向量的方向看，然後叫它朝著「正面」移動，這樣整個控制系統就完成了。

整個核心程式碼就下列三行：

Vector3 PlayerDirection = new Vector3(Input.GetAxisRaw("Horizontal"), 0, Input.GetAxisRaw("Vertical"));

transform.rotation = Quaternion.LookRotation(PlayerDirection);

PlayerController.SimpleMove(PlayerDirection * PlayerMoveSpeed);

若要實際體驗整個操控系統的話，歡迎自行下載整份專案回去玩，分享或轉載請註明本來源。
