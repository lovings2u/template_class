### Template Class

- template license: 사용했던 라이센스가 구입한 템플릿의 라이센스와 일치하는지
- Single / Multiple



1. vendor 파일에는 js, css library를 넣어두면 됩니다.

- 수정이 많이 발생하지 않을 파일들만 vendor폴더에
- custom이 필요한 부분은 application.scss 파일에서
- 혹은 custom.scss 파일 분리한 후에 `@import 'custom';` 내용 추가

2. script파일은 반드시 js plugin init 이전에 load가 끝나야 함

*app/views/layout/application.html.erb*

```erb
<%= yield :scripts %> <!-- 본문 내용중 <%= content_for :scripts %> --> 를 찾아서 대입한다.
```

*app/views/home/index.html.erb*

```erb
<% content_for :scripts do %>
<script>
	//자바스크립트 실행문
</script>
<% end %>
```

3. css파일과 js파일 이름은 asset_pipeline에 속하는 순간(vendor폴더에 들어가는 순간) precomepile되어 이름이 알수없는 이름이 됨

- 절대 원래 이름으로 찾을 수 없음(이미지도 포함)
- asset_path, font_url, image_url 등의 view helper로 원래의 이름에서 바뀐 이름이 무엇인지 연결해줘야함



4. vendor에 들어있는 파일의 숫자와 application.scss, application.js 파일에 import/require 되는 파일의 숫자가 많아지면 반드시 서버 시작 이전에 다음 과정을 수행한다.

```shell
$ rake assets:precompile
```

- 만약 프리컴파일 이후에 vendor 파일에 있는 내용물이 수정될 경우

```shell
$ rake assets:clobber # precompile 된 내용 삭제
$ rake assets:precomplie # 다시 precompile
```

- 위 과정을 수행하지 않을 경우, 사양이 낮은 컴퓨터에서는 작업이 `killed`될 수 있다.