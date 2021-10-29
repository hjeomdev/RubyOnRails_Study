# 3_Ruby on Rails : 게시판 만들기

## Rails Create와 Read 배우기
### 1. 모델 생성
``` ruby
rails generate model [모델이름]
rails g model [모델이름]
```
#### 모델 생성 시 생성되는 파일들
##### 🔆 모델 파일
- 특수한 상황에서 실행해야 하는 것을 설정하는 파일
    - 예시 : 클릭 시 조회수 올리기
##### 🔆 마이그레이션 파일 
- 실제 데이터를 넣을 형식을 결정하는 파일
##### 🔆 테스트 파일

### 2. config/migrate 폴더의 migration 파일을 수정한다

### 3. make db:migrate
migrate 파일을 토대로 테이블을 생성한다

### 4. 페이지 및 action 작성
#### 글 작성 Form 페이지
#### 글 생성 action (view는 필요없음)
#### 글 읽기 페이지

## Rails Create와 Read 실습기
#### 모델 생성
```rails generate model [모델이름]```
##### 실행예시
``` text
$ rails generate model post
Running via Spring preloader in process 68191
      invoke  active_record
      create    db/migrate/20210506181256_create_posts.rb
      create    app/models/post.rb
      invoke    test_unit
      create      test/models/post_test.rb
      create      test/fixtures/posts.yml
```
#### db/migrate/20210506181256_create_posts.rb 파일 수정
``` ruby
class CreatePosts < ActiveRecord::Migration[6.1]
  def change
    create_table :posts do |t|
      t.string :title
      t.text :content
      t.timestamps
    end
  end
end
```

#### db 반영
``` ruby
rake db:migrate
```
development.sqlite3 파일이 생성됨

#### 데이터 전달을 위해 home_controller.rb 수정
``` ruby
class HomeController < ApplicationController
    def index
        @posts = Post.all #Post라는 모델에서 모든 정보를 posts라는 인스턴스에 저장해라
    end

    def write
    end

    def create
        post = Post.new
        # 모델의 각 이름별로 데이터 저장
        post.title = params[:title] 
        post.content = params[:content]
        post.save # 저장

        redirect_to '/index' # 저장 후에 인덱스로 돌아가라
    end

end

```

#### 저장한 데이터를 확인하기 위해서 index.erb 수정
``` ruby
<% @posts.each do |post| %>
제목 : <%=post.title%> 
<br>
내용 : <%=post.content%>
<hr>
<% end %>
<br>
<a href='/write'>글쓰러가기</a>
```
##### 결과 창
``` text
12 34
글쓰러가기
```

## Rails Update와 Delete 배우기
- redirect_to "url", redirect_to :back
- 새로운 model의 메소드
    - model에는 기본적으로 id, Create_at, Update_at 컬럼이 생긴다.
    - Post.find(params[:post_id]) # post_id와 일치하는 레코드를 가져온다.
    - Post.destory(params[:post_id]) # post_id와 일치하는 레코드를 삭제한다.
### Model
#### rails g model Post
#### config/migrate폴더의 migration 파일을 수정한다
#### rake db:migrate

### Controller
#### app/controller 안에 def명령로 액션을 생성한다

### View
#### app/view/post에 생성한 액션과 같은 이름의 erb파일을 만든다

## Rails Update와 Delete 실습기
### 페이지는 하나하나 작성하기

## 스케폴딩으로 빠르게 만들기
기본적인 CRUD를 만들고 시작할 수는 없을까?
### 스케폴딩 
```rails g scaffold post content:string title:string```
``` ruby
rails g scaffold 모델이름 컬럼1이름:컬럼1타입 컬럼2이름:컬럼2타임
```
### 스케폴딩으로 생성되는 DB
| id | Content(String) | Title(String) |
| --- | --- | --- |
|  |  |  |
|  |  |  |
|  |  |  |

### 스케폴딩으로 생성되는 controller
- index
- new
- create
- edit
- update
- destroy

### 스케폴딩으로 생성되는 view
- index.html.erb
- new.html.erb
- show.html.erb
- edit.html.erb
- _form.html.erb

### 스케폴딩으로 routes.rb에 생성되는 라인
- resources :posts