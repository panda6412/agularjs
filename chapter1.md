# First Chapter\(Install codeIgniter and rest service\)

![](/assets/螢幕快照 2016-12-29 上午3.36.10.png)

首先，我們採用的是codeIgniter的php框架，載下來之後他將會是一個壓縮黨，解壓縮之後放置自己架設伺服器的目錄，這樣基本的框架就已經架設好了，再來是加入restful web service 的library 套件 ，我們將參考[https://github.com/chriskacerguis/codeigniter-restserver](https://github.com/chriskacerguis/codeigniter-restserver) 的做法

![](/assets/螢幕快照 2016-12-29 上午3.42.59.png)

載下來以後一樣是個壓縮黨，要將裡面的檔案分別放置我們的目錄裡面

![](/assets/螢幕快照 2016-12-29 上午3.43.17.png)

1. 抓取  application/libraries/Format.php 和 application/libraries/REST\_Controller.php 的檔案到我們的目錄裡

2. 再來當你要使用或建立web service 須在頂端引入檔案\(require APPPATH.'/libraries/REST\_Controller.php';\)

3. 複製 rest.php 到application/config 的目錄裡面

4. 建立一個新的類別並且繼承REST\_Controller


```php
<?php
require APPPATH.'/libraries/REST_Controller.php';
class TravelAPI extends REST_Controller
{
    public function __construct()
    {
        parent::__construct();
        $this->load->database();
    }
    public function List_get()
    {
        $query = $this->db->get('Travel');
        $this->response($query->result_array());
    }

    public function List_post()
    {
        $query = $this->db->get('Travel');
        $this->response($query->result_array());
    }
    public function ListAccount_get()
    {
        $query = $this->db->get('AccountBook');
        $this->response($query->result_array());
    }
    public function addItem_post()
    {
        $_POST = json_decode(file_get_contents('php://input'), true);

        $item = $_POST['item'];
        $amount = $_POST['amount'];
        $this->db->set('item',$item);
        $this->db->set('amount',$amount);
        $this->db->insert('AccountBook');

    }
}
?>
```

以上是我針對待會要利用angularjs 的$http service 所先準備的web service ，裡頭的內容包含4個方法

1. List\_get \(\) -&gt;使用get方式來取得Travel資料表的所有資料

2. List\_post\(\) -&gt;使用post方式取得Travel資料表的所有資料

3. ListAccounnt\_get\(\) -&gt; 使用get方式取得AccountBook資料表的所有資料

4. addItem\_post\(\) -&gt;使用post方式將資料存入至資料庫




