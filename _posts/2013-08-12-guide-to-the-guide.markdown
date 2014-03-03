---
layout: default
title: The Guide to the Guide
permalink: guide-to-the-guide
---

# Rails Girls ガイドのガイド!

*Created by H Salmon [アプリ・チュートリアル](/app)と共に*


このガイドは、最初の Rails アプリケーションを作成する[アプリ・チュートリアル](/app) を補足します。この文書の目的は、Rails アプリケーションの構造やRails の用語とコマンドについての基礎的な情報を提供することで、Rails Girls ガイドで、コードを作成する時に起こっていることを理解できるようにします。このガイドで、ワークショップのコースで学んだことを記憶にとどめたり、Rails での開発に興味を持ち続けたり、提供することを希望します。ようこそ!
### [**1.** アプリケーションを作る](#1_create_the_application)
知っておくべきコマンドについて

### [**2.** Idea の scaffold をする](#2_create_idea_scaffold)
scaffold、モデル、そして、マイグレーションについて

### [**3.** デザインする](#3_design)
デザイン層 (HTML, CSS, ERB) と MVC アーキテクチャーについて

### [**4.** 写真アップロード機能を追加する](#4_add_picture_uploads)
ライブラリ、gem、そして、オープンソースについて

### [**5.** routes を調整する](#5_finetune_the_routes)
routes と HTTP メソッド: GET, POST, PUT そして DELETE について



## <a id="1_create_the_application">*1.* アプリケーションを作る</a>

`mkdir projects` - 現在の場所（大抵はホームディレクトリ）に "projects" という*ディレクトリ* （または、フォルダ）を作成します。
`mkdir` = **m**a**k**e **dir**ectory.

`cd projects` - 先ほど作成した "projects" フォルダへ移動します。`cd` = **c**hange **d**irectory.

`rails new railsgirls` - *ワーキングディレクトリ*（現在、作業しているフォルダ）に **railsgirls** という新しい Rails アプリケーションを作成すると共に様々なフォルダを自動生成します。

`cd railsgirls` - “railsgirls” フォルダへ移動します。

`rails server` (`ruby bin¥rails server`) - あなたのコンピュータで動作するウェブサーバーを起動します。このウェブサーバーは、[http://localhost:3000](http://localhost:3000) というアドレスでアクセスすることができます。

"localhost" は、あなたのコンピュータを示します。“Localhost” refers specifically to your computer (considered the “local host”), from which a server is being launched. Localhost provides a way for developers to see their application in a browser and test the functionality while it is still in development.

## <a id="2_create_idea_scaffold">*2.* Idea の scaffold をする</a>
### Rails scaffolding とは?
ウェブアプリケーションは、多くの異なる概念、もしくは、リソースからなりたっています。（例えば、「ユーザー」、「アイデア」、「投稿」、「コメント」など）Rails scaffolding は、アプリケーションに新しいリソースを導入するコマンド（`rails generate scaffold'）です。この新しいリソースを表現したり操作したるするのに必要なコード全てを生成します。
### モデルとは?
Rails では、モデルは、アプリケーションでの単一リソースの定義とアプリケーションの他の箇所とやりとりをする方法を表現します。サイトの性質によって、これらのリソースは、ユーザー、投稿、グループといったようなものになります。モデルを生成する時に対応する*データベーステーブル*も作成されます。このデータベーステーブルは、モデルの特定の属性を表す情報を含んでいます。例えば、ユーザーモデルの場合、 'name' カラムと 'email' カラムがあるかもしれません。また、作成されるユーザーの情報はそれぞれの列に含まれるでしょう。作成するアプリケーションでは、これらのリソースとは、アイデアであり、そのモデルは、 'Idea' となります。

{% highlight rb %}
rails generate scaffold idea name:string description:text picture:string
{% endhighlight %}

アイデアモデルを作成するには、 `scaffold` コマンドを使用します。そのコマンドの引数として、モデルの名前（`idea`）の単数形とモデルの属性のパラメータ（仕様）を渡します。`idea` モデルは、`name`、`description`、そして、`picture` というコマンドで指定された属性のカラムを持つデータベーステーブルに対応するということを意味します。`scaffold` コマンドは、`id` 属性を自動的に作成します。その属性は、`主キー`と呼ばれ、データベースのテーブル間で関連を構築することに利用されます。

`rails generate scaffold` - scaffold コマンドと呼びます。

`idea` - 作成するモデルの名前を scaffold コマンドに伝えます。

`name:string description:text picture:string` - モデル（及び、関連するデータベーステーブル）に付与したい属性の一覧を提示します。引数の中の `string` や `text` は、それぞれの属性の性質を決めます。例えば、description （説明）は 'integer' とかではなく、'text' である必要があります。

### ideas テーブルの例

<table class="table table-hover table-bordered">
	<thead>
		<tr>
			<th>id</th>
			<th>name</th>
			<th>description</th>
			<th>picture</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>1</td>
			<td>“Money-spinner”</td>
			<td>“Open a moveable shop!”</td>
			<td>“GreatIdea.jpg”</td>
		</tr>
		<tr>
			<td>2</td>
			<td>“Champagne For Breakfast!”</td>
			<td>“We should do this every Friday!”</td>
			<td>“Champagne.jpg”</td>
		</tr>
		<tr>
			<td>3</td>
			<td>...</td>
			<td>...</td>
			<td>...</td>
		</tr>
	</tbody>
</table>

### 命名規約

#### Active Record
Rails で、アプリケーションのデータベースとやりとりをするデフォルトのシステムは、*Active Record* と呼ばれるシステムです。*Active Record* は、データを作成、保存、そして、検索する色々なメソッドを提供しています。データベースから情報を取り出すために *Active Record* は、命名規約を使って異なる部分同士の関連を確立します:

- テーブル名は全て小文字と単語の間を結ぶアンダースコア（\_）から成り立ちます。例、"ideas"、"invoice\_items"
- モデルは、連続した大文字と小文字が混在するという規約で命名されており、常に単数形です。例えば、テーブル名が "invoice\_items" の場合、モデル名は "InvoiceItem" になります。ですので、私たちが作成するテーブル名は、"ideas" なので、モデルは、"Idea" となります。

#### モデルの属性と型

既に説明しましたようにモデルは、対応するデータベーステーブルのカラムを表現する属性（プロパティ）を持ちます。Active Record の属性で利用できる型の一覧を以下に示します。

- `:binary` - 画像、音声、動画といったデータを格納します

- `:boolean` - 真、もしくは、偽値を格納します（あるユーザーがアプリケーションの管理者であるかをどうかを示す場合などに使用します。）

- `:date` - 日付（年、月、日）を格納します

- `:datetime` - 日付と時刻を格納します

- `:decimal` - 浮動小数点数を指定した精度とともに格納します

- `:float` - 固定小数点数を格納します（精度が必要な数学操作には、`:decimal` がむいていますが、`:float` の方が、処理が早いので精度より速度が必要な場合にむいています。）

- `:integer` - 整数を格納します

- `:primary_key` -
テーブルの主キーであることを示します。通常は、'id' です

- `:string` - 255文字までの情報を格納します （名前やメールアドレスなで）短い文に適しています

- `:text` - 文字数制限なく文字情報を格納します（例えば、 コメントやブログ投稿などに使用されます）

- `:time` - 時刻のみを格納します

- `:timestamp` - 時刻と日付を格納します。`:timestamp` は、`:datetime` と違いがあり、異なる目的に果たしますが、それについては説明しません。

### マイグレーションとは何で、どうして必要なのでしょうか？

マイグレーションは、データベースの状態を変更します。`scaffold` コマンドを実行すると関連するデータベーステーブルへの指示が含まれているマイグレーションファイルが、アプリケーションの `db/migrate` フォルダの中に加わります。例えば、`rails generate scaffold` コマンドを実行した時、'ideas' テーブルのための指示が含まれているマイグレーションが作成されました。 `rails generate model` と `rails generate migration` といった マイグレーションを作成するコマンドが他にもあります。

`rake db:migrate`
コマンドは、マイグレーションファイル内の仕様に従ってデータベースを更新します。"migrating up" として知られるこのコマンドは、idea モデルをデータベーステーブルに追加します。マイグレーションは、`rake db:rollback' コマンドで、元に戻すこともできます（"migrating down"）

## <a id="3_design">*3.* デザインする</a>
Rails アプリケーションでは、（サイトを訪問する人が目にする）ユーザーインターフェイスは、埋め込み Ruby （ERB、Embedded RuBy）を使った HTML で記述されます。ファイルは、Rails アプリケーションのディレクトリの `app` フォルダの中の 'views' にあります。

### HTML
HTML は、HyperText Markup Language
（ハイパーテキスト・マークアップ言語）の略で、ウェブページやウェブブラウザで表示する情報を作成する主言語です。HTML は、タグを使って記述されます。タグは、山括弧を用いており、（"開始タグ"と"終了タグ"の）ペアになることが多く、テキストコンテンツを含んでいます。1対のタグでは、終了タグには開き山括弧の後にスラッシュ（/）があり、開始タグと区別します。段落（HTML では文字 'p' で表現）で、使用する開始タグは `<p>` で、終了タグは `</p>` になり、表示する文章はタグの中に納められます。終了タグが必要ないペアにならないタグ（例えば、単一の改行を定義する `<br>`）は、"空要素"として知られています。ウェブブラウザは、コンテンツがどのように表示されるかを解釈するのに HTML タグを使用します。

### ERB: 埋め込み Ruby（Embedded RuBy）
ERBは、JavaScriptやHTMLといった他の言語で書かれたファイルに純粋なRubyのコードを挿入するRubyが提供するシステムです。Rubyのコードは、特殊な山括弧（`<%` と `%>`）の中に含まれ、この山括弧がコードを実行するように指示します。もし、`=` 記号があれば（`<%=` と `%>`）、中のコードが実行され、（その結果が）ページに表示されます。

例えば、アプリケーション内で、進行中のアイデアが25個ある場合、コード: `現在、進行中のアイデア: <%= Idea.count %>` は、次のように表示されます。

> 現在、進行中のアイデア: 25

### MVC アーキテクチャ
標準的な Rails
アプリケーション（あなたが作成したようなアプリケーションもそうです）では、`app/` フォルダで、代表的なのは次の3つのフォルダ（または、ディレクトリ）: （既に説明しました）‘models‘、‘controllers‘ そして ‘views‘ です。これらのディレクトリの関連は、アプリケーション、そして、Rails 開発のMVC                                       アーキテクチャとよばれる）基礎となります。

`rails generate scaffold` コマンドを実行した時、idea モデルを作成するともに付随する ideas コントローラ（`ideas_controller.rb`）を作成しました。そのファイルは、controllers フォルダ内にあります。ideas の views フォルダには、動的アプリケーションを作成するために使用するファイルが作成されています。

Rails で作成されたウェブサイトを表示しようとする時、ウェブブラウザはサーバーへリクエストを送信します。そのリクエストは、結果的に Rails *コントローラ* へ達します。*コントローラ*は、*ビュー*と*モデル*との仲介役です。*コントローラ*が情報を受け取るとアプリケーションのリソースを表現する*モデル*（今回の場合は "idea"）とやりとりをします。*モデル*から必要な情報を受け取ると*コントローラ*は、*ビュー*を使って、HTML 形式の完全なページをブラウザに送信します。

### CSS and layouts
CSS (Cascading Style Sheets) は‘マークアップ言語‘で記述されたページの書式を表現する言語です。‘マークアップ言語‘とは、タグなどの規定の書式コードを使ってテキストを処理、定義、そして、表現するための言語です。CSS は、HTML ともに利用されるのが最も一般的です。
{% highlight css %}
body { padding-top: 100px; }
footer { margin-top: 100px; }
table, td, th { vertical-align: middle; border: none; }
th { border-bottom: 1px solid #DDD; }
{% endhighlight %}

上記の CSS 内において:

`body` - この部分はセレクタと呼ばれ、装飾したい HTML 要素を参照します。this part is known as the selector and refers to the HTML element you wish to style.
`{ padding-top: 100px; }` - この部分はスタイル宣言と呼ばれ、それぞれの宣言には装飾したい属性（`padding-top`）と値（`100px`）からなるプロパティがあります。スタイル宣言は常にセミコロンで終わり、複数の宣言は中括弧でまとめます。

Rails アプリケーションでは、デフォルトのレイアウトファイルは、`application.html.erb` で、views ディレクトリの中の layouts フォルダにあります。このファイルで、アプリケーションの全てのページのデフォルトの体裁を作成することができます。

{% highlight html %}
<link rel="stylesheet" href="http://railsgirls.com/assets/bootstrap.css">
{% endhighlight %}

上記のコードにおいて、`link rel` (link relation) は、`href` (hypertext reference) 属性で指定されるコンテンツを取得するURL の性質を定義します。この引数は、要求している外部ソースはスタイルシートで、ウェブブラウザは、ページを正しく表示するためにこのファイルを取得する必要があります。

{% highlight erb %}
<%= stylesheet_link_tag "application" %>
{% endhighlight %}

このコードはスタイルシートリンクタグを返します。この場合、"application"、例えば、`application.css` になります。これは、`application.css` で実装する装飾は、アプリケーションの様々なページに適用されることになります。


{% highlight erb %}
<div class="container">
  <%= yield %>
</div>
{% endhighlight %}

このコードでは:

- HTML `div` タグは、コードを分割します。
- *container クラス*は、`div` タグ内の全てにさらなる装飾を追加します。
- `<%= yield %>` 引数は、各ページ特有のコンテンツを`div` コンテナ内へ挿入します。これによって、各ページで内容が異なってもアプリケーションでレイアウト全体が一貫するようにできます。

## *4.* Add picture uploads

### Libraries
Many programming languages, including Ruby, use a wide range of libraries. In Ruby’s case, most of these libraries are released in the form of self-contained packages called *gems*, which contain all the information required to install and implement them. These gems are contained in your application’s `Gemfile` and if you look in this file you’ll notice that when you created your first Rails application it came with several gems that ensure your application functions correctly.

Gems help simplify and prevent repetition in a developer’s code, in keeping with the DRY (Don’t Repeat Yourself) principle of software development. Gems may solve specific problems, add specific functionality, or address specific requirements, meaning that should another developer encounter a similar scenario, instead of writing new code, they can install a gem containing pre-written code. For example, “CarrierWave”, the gem you are adding to your gemfile is designed to make it easy to upload files to your application.

“Bundler” is the software Ruby uses to track and manage gems. The `bundle` command runs Bundler and installs the gems specified in your Gemfile. You’ll notice the code `source 'https://rubygems.org'` at the top of your Gemfile. Whenever you add a gem to your gemfile and run the `bundle` command, this code tells your application to fetch the gem from [https://rubygems.org](https://rubygems.org). “RubyGems” is a Ruby-specific packaging system, the purpose of which is to simplify the creation, sharing and installation of gems.

### Open-source software

Both the Rails framework and the Ruby language are examples of open-source software. Open-source software is released under a licence which ensures universal access; anyone has the right to change, study and distribute the software. Making the source code accessible enables the establishment of a diverse, reflexive, collaborative and consequently ever-evolving interactive community of programmers who all benefit from each others’ developments.

### More HTML

The file `app/views/ideas/_form.html.erb` contains HTML code that determines the look and feel of the form used for editing and creating ideas (the `edit.html.erb` and `new.html.erb` views). A partial is a snippet of HTML and Ruby code that can be reused in multiple locations. The form for editing existing ideas and the form for creating new ideas will look pretty much the same, so it makes sense to have one form for both files to use. If you look in these files you’ll notice that they have a customised heading (e.g. `<h1>Editing idea</h1>`) and then they simply say `<%= render 'form' %>` which tells Rails to render the partial `_form.html.erb`.

If you take a look in the `_form.html.erb` file, you will see the code `form_for` in the first line of code. This is a block used to create an HTML form. Using this block, we can access methods to put different input fields in the form.

The code we are implementing, `<%= f.file_field :picture %>`, tells Rails to create a file input on the form and map the submitted information to the ‘picture’ attribute of an ‘idea’ in our ideas database table. We changed the code from `<%= f.text_field :picture %>` to `<%= f.file_field :picture %>` because `file_field` makes it easier for the user to select the image they wish to upload.

In the code `<%= @idea.picture %>`, `@idea` is known as an *instance variable*. Instance variables are prefixed with an @ symbol and are defined in the controller action that corresponds with the view in which they are referenced. For the purposes of the code we are implementing, `@idea` is defined in the ‘show’ action of the `Ideas` controller, with the code `@idea = Idea.find(params[:id])`. This makes it available for us to use in the view `show.html.erb`. It could be defined differently in different controller actions (e.g. index or new). The code `@idea = Idea.find(params[:id])` uses the Rails `find` method to retrieve specific ideas from the database.

The code that follows the `@idea` variable (`.picture`) tells Rails to access the ‘picture’ attribute of our resource (idea). By replacing the code  `<%= @idea.picture %>` with `<%= image_tag(@idea.picture_url...)` we are using the Ruby `image_tag` *helper* which translates to an HTML `<img>` tag (used to define images in HTML) but by default retrieves images from the folder public/images, which is where our uploaded images are stored. The `image_tag` helper also allows us to insert a block of code which creates a path to an image associated with a particular idea (`@idea.picture_url`).

You will notice that within this block of code you are implementing we are also able to set a default width for each image (`:width => 600`). The final line of code `if @idea.picture.present?` tells Rails to check the corresponding database table to see whether a picture exists before rendering the code underneath.

## *5.* Finetune the routes

In a functional Rails application, there is an inbuilt system in place for translating incoming requests from the browser in order to return the intended response. This system is called *routing*. Requests from the browser are interpreted as specific HTTP methods. HTTP (Hypertext Transfer Protocol) is the protocol that defines how information (usually webpages or webpage components composed of text with hyperlinks - ‘hypertext’), is formatted and transmitted across the internet. There are four primary HTTP methods, each of which is a request to perform an operation on a specific resource (e.g. users, posts); GET, POST, PUT and DELETE. Rails’ inbuilt routing system automatically generates routes for each resource that map to specific actions (index, show, new, edit, create, update, delete) defined in the controller. So, for each of our models, there are seven related actions defined in the associated controller, `ideas_controller.rb`. These actions specify the appropriate response (a ‘method’) which is most likely to render the corresponding view, e.g. `ideas/index.html.erb`.


<table class="table table-bordered table-hover">
	<thead>
		<tr>
			<td>HTTP Method</td>
			<td>Path</td>
			<td>Action</td>
			<td>used for</td>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>GET</td>
			<td>/ideas</td>
			<td>index</td>
			<td>displaying a list of all ideas</td>
		</tr>
		<tr>
			<td>GET</td>
			<td>/ideas/new</td>
			<td>new</td>
			<td>returning an HTML form for creating a new idea</td>
		</tr>
		<tr>
			<td>POST</td>
			<td>/ideas</td>
			<td>create</td>
			<td>creating a new idea</td>
		</tr>
		<tr>
			<td>GET</td>
			<td>/photos/:id</td>
			<td>show</td>
			<td>displaying a specific photo</td>
		</tr>
		<tr>
			<td>GET</td>
			<td>/photos/:id/edit</td>
			<td>edit</td>
			<td>returning an HTML form for editing a specific photo</td>
		</tr>
		<tr>
			<td>PUT</td>
			<td>/photos/:id</td>
			<td>update</td>
			<td>updating a specific photo</td>
		</tr>
		<tr>
			<td>DELETE</td>
			<td>/photos/:id</td>
			<td>destroy</td>
			<td>deleting a specific photo</td>
		</tr>
	</tbody>
</table>


If you look in your `ideas_controller.rb` you can see these actions and the associated behaviour, and the HTTP method that corresponds with each action:

{% highlight rb %}
def show
    @idea = Idea.find(params[:id])

    respond_to do |format|
      format.html # show.html.erb
      format.json { render json: @idea }
    end
  end

  # GET /ideas/new
  # GET /ideas/new.json
{% endhighlight %}

`show` - the controller action

{% highlight rb %}
respond_to do |format|
      format.html # show.html.erb
      format.json { render json: @idea }
{% endhighlight %}

(This code is difficult to dissect with much clarity at this stage but if you persist with Rails you will get a better understanding as time progresses.)

In the above definition of the show action, Rails is using a `respond_to` helper method, which tells Rails to execute the subsequent *block* of code (the code enclosed by the `do...end` syntax). This code contains two different formatting options depending on the nature of the request. If the browser requests HTML then the HTML code contained in the view that corresponds with this controller action (`show.html.erb`) is rendered. If json is requested then the view is bypassed and limited information is provided.

`GET` - this is a comment to let us know which HTTP method is being executed.

So, URL requests, translated into HTTP methods, are mapped to controller actions which tell Rails to return a view.

When we insert the code `root :to => redirect('/ideas')` into our `config.rb`, it tells Rails to make the default root of our application [http://localhost:3000/ideas](http://localhost:3000/ideas) (note Localhost is being used as the domain because our application is still in development, when you launch your application this domain will be different). This URL contains a path (`/ideas`) which, by default, maps the URL to the ‘index’ action of our ideas controller and renders the associated view; `index.html.erb`. The code `rm public/index.html` removes (`rm`) the `public/index.html` file, containing the “Welcome Aboard” code, which was the previous default root for our application.
