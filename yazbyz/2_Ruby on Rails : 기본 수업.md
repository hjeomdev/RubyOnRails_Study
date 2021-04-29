# 2_Ruby on Rails : 기본 수업

## 1. Rails란 무엇인가?
### Rails 특징
#### MVC 아키텍처 구조
#### 관습적이지 않은 면만 정의하면 된다(CoC : Convention over Configuration)
#### 똑같은 것을 반복하지 않는 구조(DRY : Don't Repeat Yourself)

### MVC 구조? 그게 뭐죠?
#### Model
모델은 비즈니스 모델과 데이터, 데이터베이스를 다룹니다.
#### View
화면에 표시되는 로직과 데이터를 다룹니다
#### Controller
모델과 뷰를 연결시켜주면서 데이터의 흐름을 관리합니다.

### CoC란 무엇인가요?
Convention over Configuration의 약자인 CoC는 관습적이지 않은 부분만 코드를 작성하면 작동하도록 기본값을 설정하는 패러다임을 말합니다.
개발자가 최소한의 결정으로 최대의 효과를 내도록 도와주죠.

### DRY 원칙
Don't Repeat Yourself의 약자 DRY는 코드의 반복을 줄이는 원칙을 말합니다.  
반복되는 부분을 묶어서 다루면 반복되는 부분을 모두 고치지 않고 묶음코드 하나를 한번만 고쳐도 되니 편리하겠죠?

---
## 2. 기본 구조 파악하기
### 서버와 클라이언트간 데이터 교환
1. 클라이언트가 서버에 연결을 요청한다
2. 서버가 연결 준비를 마치고 클라이언트에 준비 완료 메시지를 보낸다
3. 클라이언트가 완료 메시지를 받았다는 확인 메시지를 보낸다.
4. TCP 연결 완료
5. 클라이언트가 서버에 특정 URL로 정보를 요청한다
6. 서버가 클라이언트에 프론트 요소를 보내준다.

### 클라이언트가 하는 일(브라우져)
HTML, CSS 파일을 읽어, 사용자가 사용하기 편하도록 화면상에 띄워주게 된다.
JS 파일을 읽어서 화면을 동적으로 구성해준다.

### 서버가 하는 일
Rails 서버는 MVC 패턴으로 이루어져 있기 때문에 Model, View, Controller가 서로 상호작용하여 정보를 가공하게 된다.
- Model : 어플리케이션의 데이터와 정보를 다루는 규칙을 담당한다
- View : 데이터 표현에 대한 부분을 담당한다
- Controller : Model과 View를 이어주며, 데이터 가공을 수행한다.

---

## 3. 페이지 생성하기
페이지 생성에는 3가지 조건이 맞아야 한다
- controller action이 존재한다
- action과 연결된 view 파일이 존재한다
- routes.rb에 url과 action이 연결시켜져 있다

### Controller와 action 의 생성
- 컨트롤러를 생성하려면 아래의 코드를 Bash 창에 입력해야 한다
``` ruby
rails generate controller home
```
- 이제 생성된 Controller에서 액션을 추가해보자
``` ruby
class HomeController < ApplicationController
  def index
  end
end
```
나는 index라는 이름의 액션을 만들었다

- View 파일의 생성
app/views 폴더 안에 생성된 home 폴더 안에 "index.erb" 파일을 생성하여 html 코드를 입력할 수 있다.

- routes.rb로 url과 action 연결하기
config 폴더 안에 있는 routes.rb 파일에 기본 url인 /로 접근하면 우리가 만든 index 라는 액션과 연결되도록 해보자
``` ruby
Rails.application.routes.draw do
  get '/' => 'home#index'
end
```

- Controller와 View 사이에 변수로 데이터 교환하기
인스턴스 변수를 이용하면 View 파일에 데이터를 전달하고, 출력할 수 있다.
``` ruby
class HomeController < ApplicationController
  def index
    @hello = "world"
  end
end
```
index.erb 파일
``` ruby
<%= @hello %>
```
view 파일에서 <%= %>기호는 안의 루비 문법 실행후 출력하고, <%%> 루비 문법을 실행시키기만 한다.

---
## 실습 코드
/workspace/Ruby_Coin/config/routes.rb
``` ruby
Rails.application.routes.draw do
	# 주소로 처음 들어가면 HomeController에서 index 액션을 연결해 주세요
	#root 'home#index'
	get '/' => 'home#index'
	
	get '/home' => 'home#hi'
  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
end
```

/workspace/Ruby_Coin/app/controllers/home_controller.rb
``` ruby
class HomeController < ApplicationController
	def index
	end
	
	def hi
		@show_message = true
		@message= "도망쳐"
	end
end
``` 

/workspace/Ruby_Coin/app/views/home/index.erb
``` ruby
hello world
```

/workspace/Ruby_Coin/app/views/home/hi.erb
```ruby
<p>hi hi</p>

<% if  true%>
<p>하하</p>
<% end %>

<% if  false%>
<p>호호</p>
<% end %>

<p><%= @message %></p>

<% if @show_message %>
<p><%= @message %></p>
<% end %>
```
---

## 4. Post와 Get으로 정보 전달하기
### 정보를 전달하는 여러가지 방식!
각 단계에서 정보를 전달하는 방식은 다양한데, 대표적으로 아래 방식을 먼저 배우게 될 것이다.
- 인스턴스 변수를 이용해서 전달하는 방식 (3번에서 배움)
- form 태그로 request 메시지에 담아서 보내는 방식
- params로 읽는 방식
- URL에 정해진 규칙에 따라 request 메시지에 담아서 보내는 방식

### Form 태그란 무엇인가?
form 태그는 주로 정보를 전송하기 위한 역할로 사용되며, 그 안에 input 태그와 함께 존재하게 된다.
``` html
<form method="post" action="/login">
    <input type="text" name="id">
    <input type="password" name="pw">
    <input type="checkbox" name="remember">
    <input type="submit"> 
</form>
```
일반적인 형태의 form 태그는 위와 같은 꼴을 하고 있는데, 여기서 각각의 역할과 그 속성을 암기하고 있으면 매우 좋다.

form 태그의 method 방식들
- get : 정보가 url 상에 담아 전송하는 방식
- post : 정보가 url상에 있지않고 다른곳에 담아 보낸다.

input 태그의 대표적인 type
- text: 일반적인 문자들을 받고 싶을때
- number: 숫자만을 받아야 할때
- checkbox: 체크박스 형식으로 true나 false값을 받아야할 때
- [관련 사이트](https://www.w3schools.com/html/html_form_input_types.asp)

---
## 실습 코드
### controller
/workspace/Ruby_Coin/config/routes.rb
``` ruby
Rails.application.routes.draw do
	# 주소로 처음 들어가면 HomeController에서 index 액션을 연결해 주세요
	#root 'home#index'
	get '/' => 'home#index'
	
	get '/home' => 'home#hi'
	
	# 2-4 실습
	get '/calcuratorForm' => 'home#calcuratorForm'
	# get 방식
	get 'result' => 'home#result'
	# post 방식
	post 'result' => 'home#result'
	# url
	get 'plus/:num1/:num2' => 'home#plus'
	
	
  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
end

```

/workspace/Ruby_Coin/app/controllers/home_controller.rb
``` ruby
class HomeController < ApplicationController
	def index
	end
	
	def hi
		@show_message = true
		@message= "도망쳐"
	end
	
	def calcuratorForm
	end
	
	# 파라미터
	def result
		@plus_result = params[:num1].to_i + params[:num2].to_i
	end
	
	def plus
		@plus_result = params[:num1].to_i + params[:num2].to_i
	end
end
```

/workspace/Ruby_Coin/app/controllers/application_controller.rb
오류 피하고 싶을 때(버전 5 해당)
``` ruby
class ApplicationController < ActionController::Base
	skip_before_action :verify_authenticity_token, raise: false
end
```

/workspace/Ruby_Coin/app/views/home/calcuratorForm.erb
``` ruby
<form method="get" action="/result" >
	<input type="number" name="num1" />
	+
	<input type="number" name="num2" />
	<input type="submit" value="제출" />
</form>

<br>

<form method="post" action="/result" >
	<input type="number" name="num1" />
	+
	<input type="number" name="num2" />
	<input type="submit" value="제출" />
</form>
```

### view
/workspace/Ruby_Coin/app/views/home/result.erb, /workspace/Ruby_Coin/app/views/home/plus.erb
``` ruby
<%= @plus_result %>
<br>
<a href="/calcuratorForm">돌아가기</a>
```