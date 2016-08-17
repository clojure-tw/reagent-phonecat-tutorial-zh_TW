# Step-00：基本的 app 建立

**[Browse code](https://github.com/vvvvalvalval/reagent-phonecat-tutorial/blob/step-0/src/cljs/reagent_phonecat_tutorial/core.cljs#L1) - [Live demo](http://reagent-phonecat-tutorial.s3-website-us-east-1.amazonaws.com/step-00/)**

***

## 啓始開發環境

在這裡只會用到一點點 Reagent。

執行以下命令：

```
git checkout step-0
```

現在開啓本地伺服器，執行：

```
lein figwheel
```

這將會使用 Figwheel Leiningen 插件開始一連串建置動作：

 * 將 ClojureScript 編譯成 JavaScript。
 * 啓動本地網頁伺服器來處理相關檔案。
 * 注意原始碼的變動。
 * 開啓 ClojureScript REPL。

若一切正常，你可以到 [http://localhost:3449/index.html](http://localhost:3449/index.html) 查看執行中的應用程式。你應該會看到白色的頁面上有 "Nothing here yet!" 字樣。

回到終端機，現在會看到 ClojureScript REPL 的提示符號：

```
cljs.user=>
```

試試在瀏覽器主控台印出一些訊息吧。輸入：

```
(println "Can you read me?")
```

回到瀏覽器打開主控台，你應該會看到訊息出現。

## 源碼概觀

爲了提供可生產的開發環境，專案剛建立後會包含大量的東西，這些並不需要全部都瞭解。以下快速導覽有意義的檔案：

 * `project.clj`：專案的設定集中在這個檔案，包含了相依管理，環境，profiles，建置流程等等。在這裡，只對相依管理有所着墨。在 `:dependencies` 子句中可以看到相依管理的設定，之中有可以設定網頁伺服器的函式庫與一些前端函式庫 (BootStrap CSS 函式庫，React JavaScript 函式庫以及 Reagent 這個 ClojureScript 函式庫)。
 * `resources/public/index.html`：這個網頁檔案會被我們的應用程式載入，由於應用程式中所有的畫面，會以使用 JavaScript 操作 DOM 的方式動態建立，所以這個檔案只載入腳本與樣式表。
 * `src/cljs/reagent_phonecat/core.cljs`：這是我們的 ClojureScript 原始碼檔案，之後的教學將會在這個檔案中寫些程式。接下來讓我們再看仔細。

在 `core.cljs` 的檔案末尾可以看到 `init!` 的函式定義：

```clojure
(defn init! []
  (mount-root))
``` 

這是應用程式的進入點：當編譯過後的 ClojureScript 載入之後，這個函式會被呼叫。它呼叫了 `mount-root` 函式：

```clojure
(defn mount-root "Creates the application view and injects ('mounts') it into the root element." 
  []
  (rg/render 
    [:p "Nothing here " (str "yet" "!")]
    (.getElementById js/document "app")))
```

這個函式使用 `reagent.core/render`，在頁面載入之後將動態產生的畫面安插進 DOM。表達式 `[:p {some content}]` 就是 "建立內有 {some content} 的 `<p>` DOM 元素" 的 Reagent 說法。

另外還有 `say-hello`，但是對於畫面並沒有什麼作用：

```clojure
(defn say-hello! "Greets `name`, or the world if no name specified.
Try and call this function from the ClojureScript REPL."
  [& [name]]
  (print "Hello," (or name "World") "!"))
```

你可以在 REPL 裡進入我們定義好的命名空間試試看。回到 REPL 的提示符號下，輸入：

```clojure
(in-ns 'reagent-phonecat.core)
(say-hello!)
```

你應該會看到訊息出現在主控台裡，這表示應用程式執行時，我們可以跟它有互動。

***

一切就緒，繼續往 [step 1](https://github.com/clojure-tw/reagent-phonecat-tutorial-zh_TW/blob/master/step-01.md) 