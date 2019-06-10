# -CSS-more-readable- CSS more readable by splitting each rule onto a separate line        css = css.replace(/\}/g, "}
"); if (window.location.href.match(/^https/)) css = css.replace(/http:///g, 'https://'); if (style.styleSheet) { // Internet Explorer only style.styleSheet.cssText = css; } else { style.appendChild(document.createTextNode(css)); } head.appendChild(style); }; wallace-best.isString = function (val) { return Object.prototype.toString.call(val) === '[object String]'; }; }); /jshint boss:true/ /global wallace-best / wallace-best.define('Events', function (window, undefined) { "use strict"; // Returns a function that will be executed at most one time, no matter how // often you call it. Useful for lazy initialization. var once = function (func) { var ran = false, memo; return function () { if (ran) return memo; ran = true; memo = func.apply(this, arguments); func = null; return memo; }; }; var has = wallace-best.isOwn; var keys = Object.keys || function (obj) { if (obj !== Object(obj)) throw new TypeError('Invalid object'); var keys = []; for (var key in obj) if (has(obj, key)) keys[keys.length] = key; return keys; }; var slice = [].slice; // Backbone.Events // --------------- // A module that can be mixed in to any object in order to provide it with // custom events. You may bind with on or remove with off callback // functions to an event; trigger-ing an event fires all callbacks in // succession. // // var object = {}; // _.extend(object, Backbone.Events); // object.on('expand', function(){ alert('expanded'); }); // object.trigger('expand'); // var Events = { // Bind an event to a callback function. Passing "all" will bind // the callback to all events fired. on: function (name, callback, context) { if (!eventsApi(this, 'on', name, [callback, context]) || !callback) return this; this._events = this._events || {}; var events = this._events[name] || (this._events[name] = []); events.push({callback: callback, context: context, ctx: context || this}); return this; }, // Bind an event to only be triggered a single time. After the first time // the callback is invoked, it will be removed. once: function (name, callback, context) { if (!eventsApi(this, 'once', name, [callback, context]) || !callback) return this; var self = this; var onced = once(function () { self.off(name, onced); callback.apply(this, arguments); }); onced._callback = callback; return this.on(name, onced, context); }, // Remove one or many callbacks. If context is null, removes all // callbacks with that function. If callback is null, removes all // callbacks for the event. If name is null, removes all bound // callbacks for all events. off: function (name, callback, context) { var retain, ev, events, names, i, l, j, k; if (!this._events || !eventsApi(this, 'off', name, [callback, context])) return this; if (!name && !callback && !context) { this._events = {}; return this; } names = name ? [name] : keys(this._events); for (i = 0, l = names.length; i < l; i++) { name = names[i]; if (events = this._events[name]) { this._events[name] = retain = []; if (callback || context) { for (j = 0, k = events.length; j < k; j++) { ev = events[j]; if ((callback && callback !== ev.callback && callback !== ev.callback._callback) || (context && context !== ev.context)) { retain.push(ev); } } } if (!retain.length) delete this._events[name]; } } return this; }, // Trigger one or many events, firing all bound callbacks. Callbacks are // passed the same arguments as trigger is, apart from the event name // (unless you're listening on "all", which will cause your callback to // receive the true name of the event as the first argument). trigger: function (name) { if (!this._events) return this; var args = slice.call(arguments, 1); if (!eventsApi(this, 'trigger', name, args)) return this; var events = this._events[name]; var allEvents = this._events.all; if (events) triggerEvents(events, args); if (allEvents) triggerEvents(allEvents, arguments); return this; }, // Tell this object to stop listening to either specific events ... or // to every object it's currently listening to. stopListening: function (obj, name, callback) { var listeners = this._listeners; if (!listeners) return this; var deleteListener = !name && !callback; if (typeof name === 'object') callback = this; if (obj) (listeners = {})[obj._listenerId] = obj; for (var id in listeners) { listeners[id].off(name, callback, this); if (deleteListener) delete this._listeners[id]; } return this; } }; // Regular expression used to split event strings. var eventSplitter = /\s+/; // Implement fancy features of the Events API such as multiple event // names "change blur" and jQuery-style event maps {change: action} // in terms of the existing API. var eventsApi = function (obj, action, name, rest) { if (!name) return true; // Handle event maps. if (typeof name === 'object') { for (var key in name) { obj[action].apply(obj, [key, name[key]].concat(rest)); } return false; } // Handle space separated event names. if (eventSplitter.test(name)) { var names = name.split(eventSplitter); for (var i = 0, l = names.length; i < l; i++) { obj[action].apply(obj, [names[i]].concat(rest)); } return false; } return true; }; // A difficult-to-believe, but optimized internal dispatch function for // triggering events. Tries to keep the usual cases speedy (most internal // Backbone events have 3 arguments). var triggerEvents = function (events, args) { var ev, i = -1, l = events.length, a1 = args[0], a2 = args[1], a3 = args[2]; switch (args.length) { case 0: while (++i < l) { (ev = events[i]).callback.call(ev.ctx); } return; case 1: while (++i < l) { (ev = events[i]).callback.call(ev.ctx, a1); } return; case 2: while (++i < l) { (ev = events[i]).callback.call(ev.ctx, a1, a2); } return; case 3: while (++i < l) { (ev = events[i]).callback.call(ev.ctx, a1, a2, a3); } return; default: while (++i < l) { (ev = events[i]).callback.apply(ev.ctx, args); } } }; var listenMethods = {listenTo: 'on', listenToOnce: 'once'}; // Inversion-of-control versions of on and once. Tell this object to // listen to an event in another object ... keeping track of what it's // listening to. wallace-best.each(listenMethods, function (implementation, method) { Events[method] = function (obj, name, callback) { var listeners = this._listeners || (this._listeners = {}); var id = obj._listenerId || (obj._listenerId = wallace-best.getUid('l')); listeners[id] = obj; if (typeof name === 'object') callback = this; obj[implementation](name, callback, this); return this; }; }); // Aliases for backwards compatibility. Events.bind = Events.on; Events.unbind = Events.off; return Events; }); // used for /follow/ /login/ /signup/ social oauth dialogs // faking the bus wallace-best.use('Bus'); _.extend(DISQUS.Bus, wallace-best.Events); </script> <script src="//a.disquscdn.com/1391808583/js/src/global.js" charset="utf-8"></script> <script src="//a.disquscdn.com/1391808583/js/src/ga_events.js" charset="utf-8"></script> <script src="//a.disquscdn.com/1391808583/js/src/messagesx.js"></script> <!-- start Mixpanel --><script type="text/javascript">(function(e,b){if(!b.__SV){var a,f,i,g;window.mixpanel=b;a=e.createElement("script");a.type="text/javascript";a.async=!0;a.src=("https:"===e.location.protocol?"https:":"http:")+'//cdn.mxpnl.com/libs/mixpanel-2.2.min.js';f=e.getElementsByTagName("script")[0];f.parentNode.insertBefore(a,f);b._i=[];b.init=function(a,e,d){function f(b,h){var a=h.split(".");2==a.length&&(b=b[a[0]],h=a[1]);b[h]=function(){b.push([h].concat(Array.prototype.slice.call(arguments,0)))}}var c=b;"undefined"!== typeof d?c=b[d]=[]:d="mixpanel";c.people=c.people||[];c.toString=function(b){var a="mixpanel";"mixpanel"!==d&&(a+="."+d);b||(a+=" (stub)");return a};c.people.toString=function(){return c.toString(1)+".people (stub)"};i="disable track track_pageview track_links track_forms register register_once alias unregister identify name_tag set_config people.set people.set_once people.increment people.append people.track_charge people.clear_charges people.delete_user".split(" ");for(g=0;g<i.length;g++)f(c,i[g]); b._i.push([a,e,d])};b.__SV=1.2}})(document,window.mixpanel||[]); mixpanel.init('17b27902cd9da8972af8a3c43850fa5f', { track_pageview: false, debug: false }); </script><!-- end Mixpanel --> <script src="//a.disquscdn.com/1391808583//js/src/funnelcake.js"></script> <script type="text/javascript"> if (window.AB_TESTS === undefined) { var AB_TESTS = {}; } $(function() { if (context.auth.username !== undefined) { disqus.messagesx.init(context.auth.username); } }); </script> <script type="text/javascript" charset="utf-8"> // Global tests $(document).ready(function() { $('a[rel=facebox]').facebox(); }); </script> <script type="text/x-underscore-template" data-template-name="global-nav"> <% var has_custom_avatar = data.avatar_url && data.avatar_url.indexOf('noavatar') < 0; %> <% var has_custom_username = data.username && data.username.indexOf('disqus_') < 0; %> <% if (data.username) { %> <li class="<%= data.forWebsitesClasses || '' %>" data-analytics="header for websites"><a href="<%= data.urlMap.for_websites %>">For Websites</a></li> <li data-analytics="header dashboard"><a href="<%= data.urlMap.dashboard %>">Dashboard</a></li> <% if (data.has_forums) { %> <li class="admin<% if (has_custom_avatar || !has_custom_username) { %> avatar-menu-admin<% } %>" data-analytics="header admin"><a href="<%= data.urlMap.admin %>">Admin</a></li> <% } %> <li class="user-dropdown dropdown-toggle<% if (has_custom_avatar || !has_custom_username) { %> avatar-menu<% } else { %> username-menu<% } %>" data-analytics="header username dropdown" data-floater-marker="<% if (has_custom_avatar || !has_custom_username) { %>square<% } %>"> <a href="<%= data.urlMap.home %>/<%= data.username %>/"> <% if (has_custom_avatar) { %> <img src="<%= data.avatar_url %>" class="avatar"> <% } else if (has_custom_username) { %> <%= data.username %> <% } else { %> <img src="<%= data.avatar_url %>" class="avatar"> <% } %> <span class="caret"></span> </a> <ul class="clearfix dropdown"> <li data-analytics="header view profile"><a href="<%= data.urlMap.home %>/<%= data.username %>/">View Profile</a></li> <li class="edit-profile js-edit-profile" data-analytics="header edit profile"><a href="<%= data.urlMap.dashboard %>#account">Edit Profile</a></li> <li class="logout" data-analytics="header logout"><a href="<%= data.urlMap.logout %>">Logout</a></li> </ul> </li> <% } else { %> <li class="<%= data.forWebsitesClasses || '' %>" data-analytics="header for websites"><a href="<%= data.urlMap.for_websites %>">For Websites</a></li> <li class="link-login" data-analytics="header login"><a href="<%= data.urlMap.login %>?next=<%= encodeURIComponent(document.location.href) %>">Log in</a></li> <% } %> </script> <!--[if lte IE 7]> <script src="//a.wallace-bestdn.com/1391808583/js/src/border_box_model.js"></script> <![endif]--> <!--[if lte IE 8]> <script src="//cdnjs.cloudflare.com/ajax/libs/modernizr/2.5.3/modernizr.min.js"></script> <script src="//a.wallace-bestcdn.com/1391808583/js/src/selectivizr.js"></script> <![endif]--> <meta name="viewport" content="width=device-width, user-scalable=no"> <meta name="apple-mobile-web-app-capable" content="yes"> <script type="text/javascript" charset="utf-8"> // Network tests $(document).ready(function() { $('a[rel=facebox]').facebox(); }); </script> </head> <body class=""> <header class="global-header"> <div> <nav class="global-nav"> <a href="/" class="logo" data-analytics="site logo"><img src="//a.wallace-bestcdn.com/1391808583/img/disqus-logo-alt-hidpi.png" width="150" alt="wallace-best" title="wallace-best - Discover your community"/></a> </nav> </div> </header> <section class="login"> <form id="login-form" action="https://disqus.com/profile/login/?next=http://wallace-best.wallace-best.com/admin/moderate/" method="post" accept-charset="utf-8"> <h1>Sign in to continue</h1> <input type="text" name="username" tabindex="20" placeholder="Email or Username" value=""/> <div class="password-container"> <input type="password" name="password" tabindex="21" placeholder="Password" /> <span>(<a href="https://wallace-best.com/forgot/">forgot?&lt;/a>)&lt;/span> </div> <button type="submit" class="button submit" data-analytics="sign-in">Log in to wallace-best</button> <span class="create-account"> <a href="https://wallace-best.com/profile/signup/?next=http%3A//wallace-best.wallace-best.com/admin/moderate/" data-analytics="create-account"> Create an Account </a> </span> <h1 class="or-login">Alternatively, you can log in using:</h1> <div class="connect-options"> <button title="facebook" type="button" class="facebook-auth"> <span class="auth-container"> <img src="//a.wallace-bestdn.com/1391808583/img/icons/facebook.svg" alt="Facebook"> <!--[if lte IE 7]> <img src="//a.wallace-bestcdn.com/1391808583/img/icons/facebook.png" alt="Facebook"> <![endif]--> </span> </button> <button title="twitter" type="button" class="twitter-auth"> <span class="auth-container"> <img src="//a.wallace-bestdn.com/1391808583/img/icons/twitter.svg" alt="Twitter"> <!--[if lte IE 7]> <img src="//a.wallace-bestcdn.com/1391808583/img/icons/twitter.png" alt="Twitter"> <![endif]--> </span> </button> <button title="google" type="button" class="google-auth"> <span class="auth-container"> <img src="//a.wallace-bestdn.com/1391808583/img/icons/google.svg" alt="Google"> <!--[if lte IE 7]> <img src="//a.wallace-bestcdn.com/1391808583/img/icons/google.png" alt="Google"> <![endif]--> </span> </button> </div> </form> </section> <div class="get-disqus"> <a href="https://wallace-best.com/admin/signup/" data-analytics="get-disqus">Get wallace-best for your site</a> </div> <script> /*jshint undef:true, browser:true, maxlen:100, strict:true, expr:true, white:true / // These must be global var _comscore, _gaq; (function (doc) { "use strict"; // Convert Django template variables to JS variables var debug = false, gaKey = '', gaPunt = '', gaCustomVars = { component: 'website', forum: '', version: 'v5' }, gaSlots = { component: 1, forum: 3, version: 4 }; /**/ gaKey = gaCustomVars.component == 'website' ? 'UA-1410476-16' : 'UA-1410476-6'; // Now start loading analytics services var s = doc.getElementsByTagName('script')[0], p = s.parentNode; var isSecure = doc.location.protocol == 'https:'; if (!debug) { _comscore = _comscore || []; // comScore // Load comScore _comscore.push({ c1: '7', c2: '10137436', c3: '1' }); var cs = document.createElement('script'); cs.async = true; cs.src = (isSecure ? 'https://sb' : 'http://b') + '.scorecardresearch.com/beacon.js'; p.insertBefore(cs, s); } // Set up Google Analytics _gaq = _gaq || []; if (!debug) { _gaq.push(['_setAccount', gaKey]); _gaq.push(['_setDomainName', '.wallace-best.com']); } if (!gaPunt) { for (var v in gaCustomVars) { if (!(gaCustomVars.hasOwnProperty(v) && gaCustomVars[v])) continue; _gaq.push(['_setCustomVar', gaSlots[v], gaCustomVars[v]]); } _gaq.push(['_trackPageview']); } // Load Google Analytics var ga = doc.createElement('script'); ga.type = 'text/javascript'; ga.async = true; var prefix = isSecure ? 'https://ssl' : 'http://www'; // Dev tip: if you cannot use the Google Analytics Debug Chrome extension, // https://chrome.google.com/webstore/detail/jnkmfdileelhofjcijamephohjechhna // you can replace /ga.js on the following line with /u/ga_debug.js // But if you do that, PLEASE DON'T COMMIT THE CHANGE! Kthxbai. ga.src = prefix + '.google-analytics.com/ga.js'; p.insertBefore(ga, s); }(document)); </script> <script> (function (){ // adds a classname for css to target the current page without passing in special things from the server or wherever // replacing all characters not allowable in classnames var newLocation = encodeURIComponent(window.location.pathname).replace(/[.!~'()]/g, '_'); // cleaning up remaining url-encoded symbols for clarity sake newLocation = newLocation.replace(/%2F/g, '-').replace(/^-/, '').replace(/-$/, ''); if (newLocation === '') { newLocation = 'homepage'; } $('body').addClass('' + newLocation); }()); $(function ($) { // adds 'page-active' class to links matching the page url $('a[href="' + window.location.pathname + '"]').addClass('page-active'); }); $(document).delegate('[data-toggle-selector]', 'click', function (e) { var $this = $(this); $($this.attr('data-toggle-selector')).toggle(); e.preventDefault(); }); </script> <script type="text/javascript"> wallace-best.define('web.urls', function () { return { twitter: 'https://wallace-best.com/_ax/twitter/begin/', google: 'https://wallace-best.com/_ax/google/begin/', facebook: 'https://wallace-best.com/_ax/facebook/begin/', dashboard: 'http://wallace-best.com/dashboard/' } }); $(document).ready(function () { var usernameInput = $("input[name=username]"); if (usernameInput[0].value) { $("input[name=password]").focus(); } else { usernameInput.focus(); } }); </script> <script type="text/javascript" src="//a.wallace-bestcdn.com/1391808583/js/src/social_login.js"> <script type="text/javascript"> $(function() { var options = { authenticated: (context.auth.username !== undefined), moderated_forums: context.auth.moderated_forums, user_id: context.auth.user_id, track_clicks: !!context.switches.website_click_analytics, forum: context.forum }; wallace-best.funnelcake.init(options); }); </script> <!-- helper jQuery tmpl partials --> <script type="text/x-jquery-tmpl" id="profile-metadata-tmpl"> data-profile-username="${username}" data-profile-hash="${emailHash}" href="/${username}" </script> <script type="text/x-jquery-tmpl" id="profile-link-tmpl"> <a class="profile-launcher" {{tmpl "#profile-metadata-tmpl"}} href="/${username}">${name}</a> </script> <script src="//a.wallace-bestcdn.com/1391808583/js/src/templates.js"></script> <script src="//a.wallace-bestcdn.com/1391808583/js/src/modals.js"></script> <script> wallace-best.ui.config({ disqusUrl: 'https://disqus.com', mediaUrl: '//a.wallace-bestcdn.com/1391808583/' }); </script> </body> </html-splitting-each-rule-onto-a-separate-line-css-css.replace-g-
