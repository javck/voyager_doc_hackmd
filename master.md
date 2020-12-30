# Laravel Voyager套件 V1.4中文說明文件 

## 快速指南

歡迎閱讀這份 Voyager 1.4版本文件。這份文件將教你如何去安裝.設定以及使用Voyager，並讓你獲得建構超酷帶後台的應用！反正就是炫到超乎你的想像...

![](https://i.imgur.com/nHqOnK9.png)

我們何不趕緊動起來吧！在開始安裝Voyager套件之前，你也許想要快速的了解到底什麼是Voyager，我們將在下一節為你說明。

### 什麼是Voyager

#### 它是什麼?

* 作為你Laravel應用的管理介面
* 提供更簡單的方式來為你的應用提供新增/刪除/修改/查詢資料的功能
* 提供菜單建構器功能(能為你管理應用裡頭的選單)
* 將你的檔案納入多媒體管理員
* CRUD生成器

Voyager只是為你的Laravel應用提供管理後台。你要怎麼呈現前台網頁完全是你的自由(老實說，它也不會幫你作)。你將會對你的應用保有完全的控制權，而透過Voyager能讓你輕鬆搞定以下任務：增加資料.編輯用戶.建立選單以及所有管理員該做的事情(更棒的是，Voayger還賦予你極高的客製彈性來擴展你的後台功能)  

#### 它不是什麼?

* 一個內容管理系統(CMS)
* 一個Blog平台
* Wordpress

Voyager並非CMS或者是Blog平台，但是你還是可以使用它來打造CMS系統又或者是Blog平台，不過Voyager設計的初衷並不只是要作這兩件事情。就如同前一節所說，導入Voyager你仍將具有應用的完全控制權，你可以決定要使用它來實現怎樣的應用。
透過Laravel和Voyager套件的完美組合，你能夠建立任何心中所想的應用，並讓開發過程變得更快而且簡單。

### 套件需求

在安裝Voyager套件到你的Laravel應用之前，請確保你的Laravel是以下的版本：
* Laravel 6
* Laravel 7
* Laravel 8

除此之外，也要確定你使用的PHP版本是7.3或更新的版本

### 安裝套件
安裝Voyager套件是非常容易的，你可以使用以下指令來為你的Laravel應用加入Voyager套件

`composer require tcg/voyager`

接下來，請確保建立一個新的資料庫並在.env檔案裡頭加入驗證相關參數(帳號.密碼.資料庫名稱等等)，如果之前就有建則是可以直接使用。除此之外，也請確保在.env檔案裡頭有APP_URL這個參數，並且這個網址應該指向到應用的首頁

```
APP_URL=http://localhost
DB_HOST=localhost
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=secret
```

最後，我們就能夠開始安裝Voyager套件了。你能夠選擇在安裝時要不要連同假資料一併生成。假資料將會包含一個管理員帳號(假如沒有用戶資料的話)，1個示範頁面，4個示範文章，2個分類以及7個設定。
如果要安裝套件但不生成假資料，只需要輸入以下指令：

`php artisan voyager:install`

假如你傾向於安裝套件一併生成假資料，則改輸入以下指令：

`php artisan voyager:install —with-dummy`

>警告：當出現 Specified key was too long error 這樣的錯誤訊息...
> 當你看到這樣的錯誤訊息代表你使用的是舊版的MySQL，你可以使用這個網址的解決方案： https://laravel-news.com/laravel-5-4-key-too-long-error

這樣就差不多完成囉!
你可以使用 php artisan serve 來開啟一個本地開發伺服器，並且開啟瀏覽器輸入網址 http://localhost:8000/admin來訪問後台
假如你有生成假資料的話，將會生成一個管理員帳號，你可以使用以下口令來進行登入

```
email: admin@admin.com
password: password
```


> 💡快速筆記
> 假資料管理員帳號只有在你的資料庫沒有任何用戶時才會產生


假如你沒有生成假資料帳戶，你或許會希望能分配管理員權限到一個已存在的用戶，你可以透過以下指令來輕鬆做到這點：

`php artisan voyager:admin your@email.com`

假如你想要建立一個新的管理員用戶，你可以加上 --create參數，像這樣：

`php artisan voyager:admin your@email.com - -create`

接下來在終端機就會以互動問答的方式來要求你輸入帳戶資料

### 安裝進階知識

這一節是針對那些想要安裝Voyager套件到已經存在的舊Laravel專案，或者是想要自己進行手動安裝的用戶。假如這不是你的情況，你可以退回上一節又或者是跳過這一節。

第一件事情要做的是要把Voyager的素材檔案佈署到專案裡頭，你能夠透過以下指令輕鬆做到這一點。

```
php artisan vendor:publish - -provider=“TCG\Voyager\VoyagerServiceProvider"
php artisan vendor:publish - -provider=“Intervention\Image\ImageServiceProviderLaravelRecent"
```

下一步，呼叫 php artisan migrate 來遷移所有Voyager的表格

> 💡快速筆記
> 假如你有需要修改Migration檔案，比如你想要使用其他表格而非users來儲存用戶資料，不要進行遷移。相對的，將Voyager的Migration檔案複製進到database/migrations，進行你的修改，然後關閉設定選項database.autoload_migrations，最後才進行遷移


現在，開啟你的User模型(通常是app/User.php 又或者是app/Models/User.php)，並讓這個類別改為繼承\TCG\Voyager\Models\User而非原先的Authenticatable

![](https://i.imgur.com/4oLpbJJ.png)

在下一步，你需要加入Voyager路由到你的routes/web.php檔案裡頭

![](https://i.imgur.com/bjkoY6b.png)

現在呼叫 php artisan db:seed --class=VoyagerDatabaseSeeder來生成需要的資料

呼叫 php artisan hook:setup 來安裝 hooks系統

呼叫 php artisan storage:link來建立 storage捷徑到public資料夾內

最後，呼叫 composer dump-autoload來完成你的安裝!


### 設定
當你安裝完Voyager，你將能找到一個新的設定檔案 config/voyager.php。在這個檔案裡頭你將能夠找到各類的選項來讓你修改Voyager的安裝設定。

> 💡快速筆記
> 假如你發現設定檔被快取了，請記得在修改完設定之後，呼叫 php artisan config:clear


接下來我們將會好好的來看這個設定檔，並對這些設定選項加以詳細說明。

#### 使用者

![](https://i.imgur.com/FYqiXkx.png)

**add_default_role_on_register:** 是否要在建立一個新使用者時為其賦予一個預設的角色
**default_role:** 預設的角色名稱
**admin_permission:** 要能夠進到後台所需要具備的權限
**namespace:** 應用的使用者模型的命名空間 
**redirect:** 當使用者登入之後要轉址到哪裡

---

#### 控制器

![](https://i.imgur.com/IbTsf2D.png)

你能夠指定Voyager套件的預設控制器命名空間。假如你想要複寫任何一個Voyager的核心控制器，你能夠複製Voyager的控制器檔案，並修改這個設定到你自定義控制器所在的位置


> 💡複寫單一控制器
> 假如你只想要複寫單一個控制器，你可以考慮加入以下程式碼到你的AppServiceProvider類別裡頭的register方法。
> $this->app->bind(VoyagerBreadController::class,MyBreadController::class);

---

#### 模型

![](https://i.imgur.com/uYXHIvt.png)

你能夠指定你模型的命名空間又或者是位置。這將在Voyager的資料庫管理員為你建立Model模型時會用到，如果不設定的話舊會使用預設的應用命名空間。

---

#### 素材

![](https://i.imgur.com/gArqIli.png)

你可能想要去指定不同的素材路徑，比如你的網站是放在子資料夾之下就需要把該資料夾作為路徑的起始。又比如假如你想要複寫這些素材以進行客製的話也需要設定這個

> 💡升級Voyager套件
> 當升級到更新的Voyager版本時，座落於/vendor/tcg/voyager/assets資料夾的素材們就需要作複寫。因此當你客製好了新的素材樣式之後，就需要把新素材所在位置設定在這裡

---

#### 儲存

![](https://i.imgur.com/4xVpnAZ.png)

預設Voyager套件使用public本地儲存庫。你能夠指定去使用任何一個在你的config/filesystems.php裡頭所設定的儲存機制。這意味著你可以使用S3.Google Cloud Storage或者任何一個你想要使用的儲存系統

---

#### 資料庫

![](https://i.imgur.com/CyUEjBv.png)

你可能想要在Voyager的資料庫管理員隱藏一些表格。在這個地方你可以指定哪些表格需要被隱藏。至於autoload_migrations允許你在執行 php artisan migrate 指令時去排除Voyager的遷移檔

---

#### 多語系

![](https://i.imgur.com/N3ySefQ.png)

你能夠設定是否要開啟多語系功能。除此之外，你也能夠去指定預設使用的語言以及所要需要支持的語系。
這裡可以了解更多的多語系資訊[這裡](https://voyager-docs.devdojo.com/core-concepts/multilanguage).

---

#### 資訊面板

![](https://i.imgur.com/11zANXS.png)

在資訊面板的設定部分，你可以加入navbar_items ，讓 data_tables支援responsive，並且管理你的所有widget

* 'navbar_items'讓你編輯用戶下拉選單的各個項目，比如 'route'用來決定該項目的連結, 'icon_class'設定該項目的Icon圖案
* 'target_blank'則決定了點擊後是否要開啟新的視窗
* (未證實)如果你把data_tables的responsive設為true的話，就可以讓datatables的顯示支持RWD

widgets這邊你能管理所有顯示在資訊面板的widget。如果不曉得該怎麼撰寫widget類別，你可以到 tcg/voyager/src/Widgets這個資料夾來參考範例Widget類別是怎麼寫的

---

#### 主色系

![](https://i.imgur.com/YWUH8gy.png)

Voyager管理資訊面板的主色系是淡藍色，你能夠透過修改 primary_color屬性來設成你想要的顏色

---

#### 顯示開發者提示

![](https://i.imgur.com/khNObGS.png)

在Voyager的管理後台有不少的開發者提示訊息又或者是通知來告訴你該如何去取得某些Voyager參數。如果你不想要顯示這些提示的話，可以把 show_dev_tips設為false

---

#### 額外的樣式

![](https://i.imgur.com/5QXxDZf.png)

你能夠加入你自定義的樣式表用來調整Voyager後台，這表示你能夠為Voyager後台設計出全新的主題風格。
如需了解更多，可以參考 [這裡](https://voyager-docs.devdojo.com/customization/additional-css-js).

---

#### 額外的JS腳本

![](https://i.imgur.com/mWAVOcS.png)


就跟先前的額外樣式相同。你可以加入你自己的JS腳本用以在Voyager後台執行，你可以根據你的需要加入多個腳本進去
如需了解更多，可以參考 [這裡](https://voyager-docs.devdojo.com/customization/additional-css-js).

---

#### Google地圖

![](https://i.imgur.com/3sb76bU.png)


這裡有一個新的資料類型 coordinates，它允許你去加入一個Google地圖。使用者能夠透過拖拉地圖上大頭釘的方式來將經緯度存到資料庫裡頭。
在這個設定你能夠設定Google地圖Key以及中心點座標。預設是把這些設定放在.env檔案裡頭。

---

#### 允許的Mimetypes

要允許哪些檔案能夠透過多媒體管理員來進行上傳，你可以定義 allowed_mimetypes

![](https://i.imgur.com/hfnlzdY.png)

使用者只能夠上傳檔案格式有列在裡頭的檔案類型之內容，如果需要允許所有類型的內容，可改用以下設定

![](https://i.imgur.com/m9OhwUT.png)

## BREAD

### 介紹

當加入或編輯某個當前資料庫表格的BREAD，你將會看到它的BREAD資訊。裡頭允許你去設定顯示名稱.Slug.Icon.模型.控制器的命名空間以及政策名稱。你也能夠選擇是否要生成該BREAD的權限

![](https://i.imgur.com/EV7rKEy.png)

當你把畫面往下滑時你將會看到表格的每一個欄位，在這裡你將能夠設定各欄位在各個頁面是否要呈現或編輯

BROWSE (欄位將會出現在顯示表格所有資料的頁面)
READ (欄位將會出現在顯示某一筆資料的頁面)
EDIT (欄位將會出現在編輯某一筆資料的頁面，並允許你去更新該欄位資料)
ADD (欄位將會出現在新增一筆資料的頁面)
DELETE (尚不清楚作用為何)

You may also choose to specify what form type you want to use for each field. This can be a TextBox, TextArea, Checkbox, Image, and many other types of form elements.
Each field also has additional details or options that can be included. These types are checkbox, dropdown, radio button, and image.
Validation
Inside of the Optional Details section for each row in your BREAD you can also specify validation rules with some simple JSON. Here is an example of how to add a validation rule for required and max length of 12
{
    "validation": {
        "rule": "required|max:12"
    }
}
Additionally, you may wish to add some custom error messages which can be accomplished like so:
{
    "validation": {
        "rule": "required|max:12",
        "messages": {
            "required": "This :attribute field is a must.",
            "max": "This :attribute field maximum :max."
        }
    }
}
You can also define multiple rules the following way:
{
    "validation": {
        "rule": [
            "required",
            "max:12"
        ]
    }
}
Action specific rules
You can define separate validation rules for edit and add:
{
    "validation": {
        "rule": "min:3",
        "edit": {
            "rule": "nullable"
        },
        "add": {
            "rule": "required"
        }
    }
}
You can find a list of all available validation rules in the Laravel docs.
Tagging
Tagging gives you the possibility to add new items to a Belongs-To-Many relationship directly when editing or adding a BREAD.
To activate this function, you simply have to enable Tagging in the relationship details

After that you can enter a free-text into the select and hit enter to save a new relationship.
Be aware:
This only stores the display-column so you have to make sure that all other fields are either nullable or have a default value.
Ordering Bread Items
You can define the default order for browsing BREADs and order your BREAD items with drag-and-drop.
For this you need to change the settings for your BREAD first:

Order column is the field in your table where the order is stored as an integer.
Order display column is the field which is shown in the drag-drop list.
Order direction the direction in which the field is ordered.
After this you can go to your BREAD-browse page and you will see a button Order.
Clicking this button will bring you to the page where you can re-arrange your items:

Scope browse-results
If you want to filter the browse results for a BREAD you can do so by creating a Scope in your model. For example if you want to only show posts that were created by the current user, define a Scope like the following:
<?php
public function scopeCurrentUser($query)
{
    return $query->where('author_id', Auth::user()->id);
}
Next, go to the BREAD-settings for posts and look for the Scope input and select currentUser:

After hitting Submit you will only see your own posts.



