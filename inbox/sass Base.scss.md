#sass

---
2021-09-01

# Base.scss

こんなの使ってたので、もらって学ぶ。

_base.scss

```scss
$breakpoints: (
  'md': (min-width: 768px),
  'lg': (min-width: 1024px),
  'xl': (min-width: 1200px),
  'xxl': (min-width: 1400px)
) !default;

@mixin responsive($breakpoint) {
  @if map-has-key($breakpoints, $breakpoint) {
    @media screen and #{inspect(map-get($breakpoints, $breakpoint))} {
      @content;
    }
  }
 
  // マップ型で定義されていない値が呼び出された時はエラーを返す
  @else {
    @error "指定されたブレークポイントは定義されていません。" + "指定できるブレークポイントは次のとおりです。 -> #{map-keys($breakpoints)}";
  }
}

@mixin retina {
  @media
    only screen and (-webkit-min-device-pixel-ratio: 2),
    only screen and (min--moz-device-pixel-ratio: 2),
    only screen and (-o-min-device-pixel-ratio: 2/1),
    only screen and (min-device-pixel-ratio: 2),
    only screen and (min-resolution: 192dpi),
    only screen and (min-resolution: 2dppx) {
    @content;
  }
}

* {
  margin: 0;
  padding: 0;
  background-position: center center;
  box-sizing: border-box;
  outline: none;
}

img {
  height: auto;
}

html {
  -webkit-text-size-adjust: 100%;
  height: 100%;
	font-size: 62.5%;
}


body {
  font-weight: 400;
  color: [[364D3B]];
  font-size: 1.6rem;
  line-height: 2;
  margin: auto;
  text-align: left;
  height: 100%;
  letter-spacing: 0.05em;
  line-break: strict;
  word-break:break-word;
  overflow-y: auto;
}

body, .toggle p, .about_concept p {
  font-family: 'A-OTF UD新丸ゴ Pro','ヒラギノ丸ゴ ProN','Hiragino Maru Gothic ProN','Hiragino Kaku Gothic ProN','ヒラギノ角ゴ ProN W3','メイリオ', Meiryo, sans-serif;
}

li {
  list-style: none;
}

img {
	width: 100%;
  height: auto;
}

a {
  text-decoration: none;
  color: [[364d3b]];
  transition: 0.3s ease-in-out;
  cursor: pointer;

	&:visited {
		text-decoration: none [[364d3b]];
	}
	
	&:hover {
		text-decoration: none;

		img {
			opacity: 0.8;
		}
	}

	img {
		transition: 0.3s ease-in-out;
	}

  .event-none {
    pointer-events: none;
  }
}

.tlink-underline a {
  border-bottom: 1px solid [[364d3b]];
  :hover {
    color: [[619f6e]];
    border-color: [[619f6e]];
  }
}

.tlink-active-color a {
  &:hover {
    color: [[619f6e]];
  }
}

.elink {
  border-bottom: 1px solid [[364d3b]];
  :hover {
    color: [[619f6e]];
    border-color: [[619f6e]];
  }
}




*::selection {
  background: [[14274c]];
  color: #f;
}

p,
.post_body ul,
.info_body ul,
.about_about table,
.noshi {
  text-align: justify;
}

img {
  border: 0;
  vertical-align: top;
}

h1, h2, h3, h4 {
  letter-spacing: 0.04em;
  font-size: 1.8rem;
}

table {
  border-collapse: collapse;
}

.left {
  float: left;
}

.right{
  float: right;
}

.align-right {
  text-align: right;
}

.clearfix:after {
  content: ".";
  /* 新しいコンテンツ */
  display: block;
  clear: both;
  height: 0;
  visibility: hidden;
  /* 非表示に */
}

.clearfix {
  min-height: 1px;
}

* html .clearfix {
  height: 1px;
  /*/*/
  /*/
  height: auto;
  overflow: hidden;
  /**/
}
.sp_only {
  display: none;
}

.pc_only {
  display: block;
}

.inner {
	max-width: 1280px;
	width: 100%;
	padding: 0 6%;
	margin: 0 auto;
}

.flex {
	display: flex;
  flex-flow: column wrap;
  justify-content: center;
  > * {
    width: 100%;
	}
	&.row {
		flex-flow: row wrap;
		justify-content: space-between;
		> * {
			width: auto;
		}
    &.reverse {
      flex-flow: row-reverse wrap;
    }
	}
  @include responsive(md) {
    display: flex;
    flex-flow: row wrap;
    justify-content: space-between;
    > * {
      width: auto;
    }
  }
}

.harf.row .box {
	width: 47%;
}

@include responsive(md) {
	.harf .box {
		width: 47%;
	}
}



/*----------------------------------------------------------------------------------

	レイアウト

----------------------------------------------------------------------------------*/

.max_wrapper {
	width: 100%;
	max-width: 1920px;
	position: relative;
	margin: 0 auto;
  background: [[f5f4f1]];
}

.wide_1080 {
	max-width: 1280px;
	width: 100%;
	padding: 0 6%;
	margin: 0 auto;
}

.wide_795 {
	max-width: 968px;
	width: 100%;
	padding: 0 6%;
	margin: 0 auto;
}

.wide_700 {
	max-width: 920px;
	width: 100%;
	padding: 0 6%;
	margin: 0 auto;
}

.wide_620 {
	max-width: 620px;
	width: 100%;
	padding: 0;
	margin: 0 auto;
}

.wide_1200 {
	max-width: 1480px;
	width: 100%;
	padding: 0 8%;
	margin: 0 auto;
}

.layout_half {
	display: flex;
	justify-content: space-between;
	.layout {
		width: 47%;
	}
}

.line_deco {
	background: url(images/common/line_deco.png) repeat-x left top;
	background-size: 30px auto;
}

.arrow {
	line-height: 1.4;
	font-size: 1.4rem;
	padding: 2px 34px 3px 0;
	background: url(images/common/arrow_right.png) no-repeat right 3px top 0.30em;
	background-size: 23px auto;
	position: relative;
  overflow: hidden;
  display: inline-block;
  transition: 0.2s ease-in-out;
	&:hover {
		background-position: right 0 top 0.30em;
	}
}

.arrow_area {
	text-align: right;
}
.align-center {
	text-align: center;
}
.align-left {
	text-align: left;
}
.align-right {
	text-align: right;
}

.site-main {
  margin-top: -20px;
}
```


使い方。

```scss
@use 'base' as b


@include b.retina {
	background: url(.....);
	background-size 100% auto;
}


@include b.responsive(md) {
	padding: 15rem 0 10rem;
}

```

@use でとりこんで、 @include で指定して使うのか。

responsive は breakpoint が設定してある。

md とか xl でその大きさのときの設定が出来るスグレモノだ。

retina はまんま include  して設定すれば、retina サイズのときに適応されるのか、賢い。


