/*
* jQuery pager plugin
* Version 1.0 (12/22/2008)
* @requires jQuery v1.2.6 or later
*
* Example at: http://jonpauldavies.github.com/JQuery/Pager/PagerDemo.html
*
* Copyright (c) 2008-2009 Jon Paul Davies
* Dual licensed under the MIT and GPL licenses:
* http://www.opensource.org/licenses/mit-license.php
* http://www.gnu.org/licenses/gpl.html
* 
* Read the related blog post and contact the author at http://www.j-dee.com/2008/12/22/jquery-pager-plugin/
*
* This version is far from perfect and doesn't manage it's own state, therefore contributions are more than welcome!
*
* Usage: .pager({ pagenumber: 1, pagecount: 15, buttonClickCallback: PagerClickTest });
*
* Where pagenumber is the visible page number
*       pagecount is the total number of pages to display
*       buttonClickCallback is the method to fire when a pager button is clicked.
*
* buttonClickCallback signiture is PagerClickTest = function(pageclickednumber) 
* Where pageclickednumber is the number of the page clicked in the control.
*
* The included Pager.CSS file is a dependancy but can obviously tweaked to your wishes
* Tested in IE6 IE7 Firefox & Safari. Any browser strangeness, please report.
* 
*
*
* sunxuefeng modified on 2011-01-25
*/
(function ($) {
    
    $.fn.pager = function (options) {

        var opts = $.extend({}, $.fn.pager.defaults, options);

        return this.each(function () {

            // empty out the destination element and then render out the pager with the supplied options
            $(this).empty().append(renderpager(parseInt(opts.pagenumber), parseInt(opts.pagecount), opts.buttonClickCallback, opts)); //sunxuefeng modified on 2011-01-25

            // specify correct cursor activity
            $('.pages li').mouseover(function () { document.body.style.cursor = "pointer"; }).mouseout(function () { document.body.style.cursor = "auto"; });
        });
    };

    // render and return the pager with the supplied options
    function renderpager(pagenumber, pagecount, buttonClickCallback, options) {

        // setup $pager to hold render
        var $pager = $('<ul class="pages"></ul>');

        // add in the previous and next buttons
        //sunxuefeng modified on 2011-01-25
        $pager.append(renderButton('first', pagenumber, pagecount, buttonClickCallback, options)).append(renderButton('prev', pagenumber, pagecount, buttonClickCallback, options));

        // pager currently only handles 10 viewable pages ( could be easily parameterized, maybe in next version ) so handle edge cases
        var startPoint = 1;
        var endPoint = 9;

        if (pagenumber > 4) {
            startPoint = pagenumber - 4;
            endPoint = pagenumber + 4;
        }

        if (endPoint > pagecount) {
            startPoint = pagecount - 8;
            endPoint = pagecount;
        }

        if (startPoint < 1) {
            startPoint = 1;
        }

        // loop thru visible pages and render buttons
        for (var page = startPoint; page <= endPoint; page++) {

            var currentButton = $('<li class="page-number">' + (page) + '</li>');

            page == pagenumber ? currentButton.addClass('pgCurrent') : currentButton.click(function () { buttonClickCallback(this.firstChild.data); });
            currentButton.appendTo($pager);
        }

        // render in the next and last buttons before returning the whole rendered control back.
        //sunxuefeng modified on 2011-01-25
        $pager.append(renderButton('next', pagenumber, pagecount, buttonClickCallback, options)).append(renderButton('last', pagenumber, pagecount, buttonClickCallback, options));

        return $pager;
    }

    // renders and returns a 'specialized' button, ie 'next', 'previous' etc. rather than a page number button
    //sunxuefeng modified on 2011-01-25
    function renderButton(buttonLabel, pagenumber, pagecount, buttonClickCallback, options) {

        var labelText = "";
        var destPage = 1;

        // work out destination page for required button type
        switch (buttonLabel) {
            case "first":
                destPage = 1;
                labelText = options.firstText;
                break;
            case "prev":
                destPage = pagenumber - 1;
                labelText = options.prevText;
                break;
            case "next":
                destPage = pagenumber + 1;
                labelText = options.nextText;
                break;
            case "last":
                destPage = pagecount;
                labelText = options.lastText;
                break;
        }

        var $Button = $('<li class="pgNext">' + labelText + '</li>');

        // disable and 'grey' out buttons if not needed.
        if (buttonLabel == "first" || buttonLabel == "prev") {
            pagenumber <= 1 ? $Button.addClass('pgEmpty') : $Button.click(function () { buttonClickCallback(destPage); });
        }
        else {
            pagenumber >= pagecount ? $Button.addClass('pgEmpty') : $Button.click(function () { buttonClickCallback(destPage); });
        }

        return $Button;
    }

    // pager defaults. hardly worth bothering with in this case but used as placeholder for expansion in the next version
    $.fn.pager.defaults = {
        pagenumber: 1,
        pagecount: 1,
        //sunxuefeng modified on 2011-01-25
        firstText: "FIRST",
        prevText: "PREV",
        nextText: "NEXT",
        lastText: "LAST",
        buttonClickCallback: function (pageclickednumber) {
            var url = location.href.split("?");
            if (url.length > 1) {
                var querylist = url[1].split("&");
                var query;
                var newUrl = "";
                for (var i = 0; i < querylist.length; i++) {
                    query = querylist[i].split("=");
                    if (query.length > 1 && query[0] != "page")
                        newUrl += "&" + query[0] + "=" + query[1];
                }
                newUrl += "&page=" + pageclickednumber;
                if (newUrl.length > 0)
                    newUrl = "?" + newUrl.substr(1, newUrl.length - 1);
                location.href = url[0] + newUrl;
            }
            else
                location.href = url[0] + "?page=" + pageclickednumber;
        }

    };

})(jQuery);