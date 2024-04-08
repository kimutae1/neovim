# Nvim 설치 가이드
vi는 잘 쓰면 많은 생산성의 증가를 가져 온다.  \
하지만 잘 쓰기까지 많은 노력이 필요 하다. \
그렇지만 엄청난 노력을 할 필요는 없다. \
다만 간단한 텍스트 작성이라도 vi로 작성을 하려는 의지는 있어야 할 것이다. \
이 기능이 있을 것 같은데? 라고 생각하면  거의 다 구현 되어 있을 것이다. \
회사에서는 잘 쓰고 있었는데 집에 와서 다시 설치를 하려 하니 \
설치 방법이 가물가물 해져서 이 기회에 확실하게 정리를 해본다.


## nvim & astornvim install

```
sudo apt -y install libfuse2 gcc
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim.appimage
chmod u+x nvim.appimage
./nvim.appimage

#nvim.appimage 파일을 /bin/vi 로 이동
sudo mv nvim.appimage /bin/nvim
git clone --depth 1 https://github.com/AstroNvim/template ~/.config/nvim
rm -rf ~/.config/nvim/.git
nvim

echo  'alias vi=nvim' > ~/.zshrc
```

## addon setting
 1. astor community 에서 탐색을 하여 본다. \
  https://github.com/AstroNvim/astrocommunity \
  좋은 기능이 많은 것 같다. 발굴해 보자 \
  예제 설정은 내가 당장 이 글을 작성하기 위한 마크다운 프리뷰라는 애드온을 설치 하는 부분을 예제로 들었다.


 2. lua/community.lua 에 import 설정 추가
```
return {
  "AstroNvim/astrocommunity",
  { import = "astrocommunity.pack.lua" },
  { import = "astrocommunity.markdown"},
   }
```
.lua는 생략해도 된다


3. markdown.lua를 작성한다.  \
파일명은 위의 import설정과 동일 하게 맞춰준다 \
해당 설정은 원하는 소스 내에 init.lua를 보면 알 수 있다. \
  { import = "astrocommunity.markdown"}, \
 https://github.com/AstroNvim/astrocommunity/blob/main/lua/astrocommunity/markdown-and-latex/markdown-preview-nvim/init.lua

```
> pwd
/home/kth/.config/nvim/lua/plugins
> ls
astrocommunity  astrolsp.lua  markdown.lua  none-ls.lua     user.lua
astrocore.lua   astroui.lua   mason.lua     treesitter.lua

```

```
> cat markdown.lua
return {
{
  "iamcco/markdown-preview.nvim",
  cmd = { "MarkdownPreviewToggle", "MarkdownPreview", "MarkdownPreviewStop" },
  build = "cd app && yarn install",
  init = function()
    vim.g.mkdp_filetypes = { "markdown" }
  end,
  ft = { "markdown" },
},
}
```

4. nvim 내에서 패키지 업데이트 하기 
[space] - [ p ] - [ u ]

5. nvim 내에서 :MarkdownPreview를 입력한다. \
  참고로 확장자가 .md 인 파일에서만 동작한다. \
  이걸 몰라서 설치가 잘 안된줄 알고 삽질 엄청 했다.


## (번외)vim register -> clipboard 저장
vi 로 저장 하면 레지스터라는  곳에 저장 되고 우리가 흔히 아는 ctrl+c , v 는 클립보드에 저장이 된다.
vi를 즐겨 쓰다 보면 이런 부분이 불편 하기에 어디서는 복불을 할 수 있도록 셋팅이 필요 하다.

```
sudo apt-get install xclip
# 또는
sudo apt-get install xsel
```

클립보드로 복사하기
일반 모드에서: "+y를 사용하여 클립보드로 복사할 수 있습니다. 예를 들어, 현재 라인을 클립보드로 복사하려면 "+yy를 사용합니다.
비주얼 모드에서: 선택한 텍스트를 클립보드로 복사하려면, 비주얼 모드에서 텍스트를 선택한 후 "+y를 사용합니다.
클립보드에서 붙여넣기
일반 모드에서: "+p를 사용하여 클립보드의 내용을 현재 위치에 붙여넣을 수 있습니다.

아래 명령어를 vi에서 실행 하거나 .vimrc에 추가 한다면 그냥 yy만 하더라도 클립보드에 복사가 된다..너무 좋다

```
:set clipboard=unnamed
```

## 여담
여담이지만 서버를 만지는 엔지니어라면 vi는 자세히 알아둬야 할 필요가 있다고 본다. \
하지만 신입 엔지니어에게 배워보라고 권유를 하기에는 라때와는 다르게 현재 엔지니어들은 정말 많은 것을 알아야 한다. \
그렇기 때문에 뭔가 강요는 잘 못하겠다. \
뭐 하다 보면 알게 되지 않을까 생각이다. 


