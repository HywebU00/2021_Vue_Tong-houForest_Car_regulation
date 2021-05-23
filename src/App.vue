<script>
import Header from"@/components/Header.vue";
import Footer from"@/components/Footer.vue";
import TabContent from"@/components/TabContent.vue";
import $ from 'jquery';

export default {
  components:{
    Header,Footer,TabContent,$
  },
  mounted(){
  $(function(){
  /*-----------------------------------*/
  ///////////////// 變數 ////////////////
  /*-----------------------------------*/
  var _window = $(window),
    ww = _window.outerWidth(),
    wh = _window.height(),
    wwNormal = 1400,
    wwMedium = 992,
    wwSmall = 768,
    wwxs = 576;
    /*-----------------------------------*/
  ////////////////多組Tab////////////////
  /*-----------------------------------*/
  var tab_headerHeight = Math.floor($(".header").outerHeight(true));
  var resizeTimer1;
 
  _window.resize(function () {
    clearTimeout(resizeTimer1);
    resizeTimer1 = setTimeout(function () {
      ww = _window.outerWidth();
      tabSet();
    }, 50);
  });

  function tabSet() {
    $(".tabs").each(function () {
      var _tab = $(this),
        _tabItem = _tab.find(".tabItem"),
        _tabContent = _tab.find(".tabContent"),
        tabwidth = _tab.width(),
        tabItemHeight = _tabItem.outerHeight(),
        tabContentHeight = _tab.find(".active").next().innerHeight(),
        tiGap = 0,
        tabItemLength = _tabItem.length,
        tabItemWidth;
      _tab.find(".active").next(".tabContent").show();
      if (ww >= wwSmall) {
        _tabContent.css("top", tabItemHeight);
        _tab.height(tabContentHeight + tabItemHeight);
        tabItemWidth = (tabwidth - (tabItemLength - 1) * tiGap) / tabItemLength;
      } else {
        _tab.css("height", "auto");
        //_tabItem.width(tabwidth);
        _tabItem.css("margin-left", 0).last().css("position", "relative");
      }
      _tabItem.focus(tabs); //改button後，前面改_tabItem
      _tabItem.click(tabs); //改button後，前面改_tabItem
      function tabs(e) {
        var _tabItemNow = $(this), //改button後，原來$(this).parent(),改$(this)
          tvp = _tab.offset().top,
          tabIndex = _tabItemNow.index() / 2,
          scollDistance = tvp + tabItemHeight * tabIndex - tab_headerHeight;
        _tabItem.removeClass("active");
        _tabItemNow.addClass("active");
        if (ww <= wwSmall) {
          _tabItem.not(".active").next().slideUp();
          _tabItemNow.next().slideDown();
          $("html,body")
            .stop(true, false)
            .animate({ scrollTop: scollDistance });
        } else {
          _tabItem.not(".active").next().hide();
          _tabItemNow.next().show();
          tabContentHeight = _tabItemNow.next().innerHeight();
          _tab.height(tabContentHeight + tabItemHeight);
        }
        e.preventDefault();
      }
    });
  }
  $(".tabs>.tabItem:first-child>a").trigger("click");
  tabSet();  
  /*-----------------------------------*/
  /////////// 無障礙快捷鍵盤組合  //////////
  /*-----------------------------------*/
  $(document).on("keydown", function (e) {
    // alt+S 查詢
    if (e.altKey && e.keyCode == 83) {
      $("html, body").animate({ scrollTop: 0 }, 200);
      $(".search").find('input[type="text"]').focus();
    }
    // alt+U header
    if (e.altKey && e.keyCode == 85) {
      $("html, body").animate({ scrollTop: 0 }, 200);
      $("header").find(".accesskey").focus();
    }
    // alt+C 主要內容區
    if (e.altKey && e.keyCode == 67) {
      $("html, body")
        .stop(true, true)
        .animate(
          { scrollTop: $(".main").find(".accesskey").offset().top - 70 },
          800
        );
      $(".main").find(".accesskey").focus();
    }
    // alt+B footer
    if (e.altKey && e.keyCode == 90) {
      $("html, body")
        .stop(true, true)
        .animate(
          { scrollTop: $("footer").find(".accesskey").offset().top },
          800
        );
      $("footer").find(".accesskey").focus();
    }
  });
  //無障礙切換slick箭頭語系
  if ($("html")[0].hasAttribute("labg")) {
    var weblang = $("html").attr("lang");
    if (weblang.substring(0, 2) == "zh") {
      $(".slick-prev").attr("title", "上一筆");
      $(".slick-next").attr("title", "下一筆");
    } else if (weblang.substring(0, 2) !== "zh") {
      $(".slick-prev").attr("title", "previous");
      $(".slick-next").attr("title", "next");
    }
  }
  // 無障礙錨點切換語系，更改accesskey的title名稱
  var weblang = $("html").attr("lang");
  if (weblang.substring(0, 2) == "zh") {
    $("header").find(".accesskey").attr("title", "上方功能區塊");
    $(".main").find(".accesskey").attr("title", "中央內容區塊");
    $("footer").find(".accesskey").attr("title", "下方功能區塊");
    $(".search").find(".accesskey").attr("title", "關鍵字搜尋：文章關鍵字搜尋");
  } else if (weblang.substring(0, 2) !== "zh") {
    $("header").find(".accesskey").attr("title", "header");
    $(".main").find(".accesskey").attr("title", "content");
    $("footer").find(".accesskey").attr("title", "footer");
    $(".search").find(".accesskey").attr("title", "search");
  }

  /*-----------------------------------*/
  ///////////////置頂go to top////////////
  /*-----------------------------------*/
    $(window).bind("scroll", function () {
    if ($(this).scrollTop() > 200) {
      $(".scrollToTop").fadeIn();
    } else {
      $(".scrollToTop").fadeOut();
    }
      });
    })
  /*-----------------------------------*/
  /////click event to scroll to top//////
  /*-----------------------------------*/
  $(".scrollToTop").click(function (e) {
    console.log($("html, body"))
    $("html, body").animate({ scrollTop: 0 }, 400);
    $("a.goCenter").focus(); //加入這行
    e.preventDefault();
  });
  $(".scrollToTop").keydown(function (e) {
     var _body = $("body");
    _body.find("a.goCenter").focus();
    e.preventDefault();
  });
  }
}
</script>
<template>
  <!-- 直接跳主內容區 -->
  <a class="goCenter" href="#center" tabindex="1">按Enter到主內容區</a>
  <div class="wrapper">
    <Header/>
        <div id="center" class="main">
       <router-view/>
        <Footer/>
    </div>
    <a href="javascript:;" class="scrollToTop">回頁首</a>
  
  </div>

</template>

<style lang="scss">
@import"~@/assets/sass/hyui.scss";
</style>
