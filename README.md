#Lively { final-exercise: Bundlin; }
##Semantics in HTML & Accessibility

Student: Tijs Luitse

##Summary:
- Semantic HTML
- Navigation
- Nesting
- Buttons
- The web is for everyone
- Expanding the clickable area

##Semantic HTML

In the case of writing semantic HTML, it’s hard to decide when to use certain HTML5 elements over others. The most important thing is that the web needs to be readable for humans and for computers. So we need to understand how both of these users read the web. This is a widely discussed topic, Divya Manian caused a stir by writing the following post in Smashing Magazine:

“Allow me to paint a picture:
You are busy creating a website.
You have a thought, “Oh, now I have to add an element.”
Then another thought, “I feel so guilty adding a div. Div-itis is terrible, I hear.”
Then, “I should use something else. The aside element might be appropriate.”
Three searches and five articles later, you’re fairly confident that aside is not semantically correct.
You decide on an article because at least it’s not a div.
You’ve wasted 40 minutes, with no tangible benefit to show for it.”

-Divya Manian

Source: [HTML5 Doctor](http://html5doctor.com/lets-talk-about-semantics/)
 
By looking at a huge project like Bundlin, it is even harder to know what the developer was thinking of that moment he/she was creating a certain element. I personally think it’s up to the opinion of the developer what element to use in what case. But there needs to be an overall writing style in the whole project. 

###Navigation

First, I noticed the use of ```<ul class=”group”>```’s in the navigation sidebar. When looking deeper into this section of the website I was noticing there was something wrong.

The HTML5 specification defines ```<nav>``` as: “The nav element represents a section of a page that links to other pages or to parts within the page: a section with navigation links. Not all groups of links on a page need to be in a nav element only sections that consist of major navigation blocks are appropriate for the nav element. In particular, it is common for footers to have a list of links to various key parts of a site, but the footer element is more appropriate in such cases, and no nav element is necessary for those links.”

So when an ```<ul>``` element which has only one listed ```<li>``` element, it isn’t a list of things. If you want to use a ```<ul>``` in the navigation, be sure that the different elements (in this case, the logo and login) are all inside different ```<li>```’s, which are inside one ```<ul>```, inside the ```<nav>```. 

```
<ul class="group">
    <li class="bln-sidebaricon bln-sidebaricon-logo">
        <a ui-sref="app.home" href="/">
            <span class="image bln-sprite bln-sprite-logo bln-sprite-logo-loaded"></span>
        </a>
    </li>
</ul>

<ul class="group group-animate ng-scope" ng-if="!user.loggedIn">
    <li class="bln-sidebaricon bln-sidebaricon-text">
        <a title="Sign in" class="text" ng-click="login()">
            <span class="bln-icon bln-icon-white bln-icon-sidebar bln-icon-budicon-23"></span>
            Sign in
        </a>
    </li>
</ul>
```

I have created a new semantic structure with all navigation elements, inside list items, inside an unordered list, inside the nav element. Now it’s possible to navigate regularly, also for users with only a keyboard. 

```
<ul>
    <li class="group bln-sidebaricon bln-sidebaricon-logo"><!-- Logo -->
        <a ui-sref="app.home">
            <figure class="image bln-sprite bln-sprite-logo {{generalLoading ? 'bln-sprite-logo-' + generalLoading : ''}}"></figure>
            <span class="bln-icon bln-icon-budicon-4" ng-if="!$state.includes('app.home')"></span>
        </a>
    </li>
    <li class="group group-animate" ng-if="user.loggedIn"><!-- User Logged in -->
        <div class="bln-sidebaricon bln-sidebaricon-avatar" ng-style="{'background-image': 'url(' + (user.picture.h128 || user.picture.original || '/images/default.png') + ')'}">
            <button ui-sref="app.view_profile.bundles({ profileScreenName: user.username })"></button>
        </div>
    </li>
    <li class="group group-animate" ng-if="!user.loggedIn"><!-- Logo Login -->
        <div class="bln-sidebaricon bln-sidebaricon-text">
            <a title="Sign in" class="text" ng-click="login()" tabindex="0">
                <span class="bln-icon bln-icon-white bln-icon-sidebar bln-icon-budicon-23"></span>
                Sign in
            </a>
        </div>
    </li>
</ul>
```

Source: [HTML5 Doctor](http://html5doctor.com/nav-element/)

###Nesting

After further research I came across more problems, when looking at the code what surprises me is that the development team has not been consistent in the use of ```<a>``` or ```<button>``` elements, ```<img>``` elements inside ```<p>```’s, and the use of ```<div>```’s to create a button or link. 

Focussing on the intro of the website, I was shocked seeing an ```<p>``` element with within two images and an <a> element.  

```
<p class="bln-credits bln-credits-small">
  	<img src="/images/intro-handcrafted.png" alt="Handcrafted by" class="handcraftedby">
  	<a href="http://lifely.nl?ref=bundlin" target="_blank" class="logo">
  		<img src="/images/logo_lifely.png" alt="Lifely">
  	</a>
</p>
```

In this case, the content inside the ```<p>``` element has nothing to do with text or textual content. To be precise it’s an image linked to another page with some kind of a header to explain the page link. In this case, I would prefer to use a ```<div>``` element. “The div element has no special meaning at all. It represents its children. It can be used with the class, lang, and title attributes to mark up semantics common to a group of consecutive elements.” -
W3C Specification.

```
<div class="bln-credits bln-credits-small">
	<img src="/images/intro-handcrafted.png" alt="Handcrafted by" class="handcraftedby">
	<a href="http://lifely.nl?ref=bundlin" target="_blank" class="logo">
		<img src="/images/logo_lifely.png" alt="Lifely">
	</a>
</div>
```

###Buttons

The web needs to be usable for every user, in what situation he or she is. People walking around with a crying baby on their arm or someone with a broken arm maybe uses only the keyboard to navigate through a website. The web also needs to be designed for them. 

When creating a clickable area, the developer needs to decide what element to use. Some people may think a ``<div>`` will do the trick, just add an event listener on the element in Javascript and the trick is done. But that’s certainly not how it’s done. Using a ``<div>`` for a clickable area is the most disgusting thing to do when developing a website. There is no semantically value in a ```<div>``` element. Screen Readers will not see it as a button, there is no standard focus state and when disabling Javascript, the ``<div>`` is worse less. 

Ok, so how do you need to choose between creating an ``<a>`` or an ``<button>`` to navigate. You only need to ask one question: What will happen when the user activates this control? In The Microsoft User Interface Interaction Guidelines lies the answer. There are two cases: Is this action to navigate to another page or used as an anchor for a section on the same page? Then use an ```<a>``` link element with a useful href. When the action of the clickable element is to create a popup or other inpage action? Use the ``<button>`` element.

```
<a title="Sign in" class="text" ng-click="login()">
	<span class="bln-icon bln-icon-white bln-icon-sidebar bln-icon-budicon-23"></span>
    Sign in
</a>
```

This is the login button from the navigation section. When the user clicks this element it creates a popup which is not necessary to open in a new window. Therefore, I would choose a ```<button>``` instead of an ```<a>``` element. 

Source: [Microsoft](https://msdn.microsoft.com/en-us/library/windows/desktop/dn742402.aspx)

###The web is for everyone

In the case of buttons, there are two main users which benefit from the ```<button>``` element being used properly. These are screen reader and keyboard users. The ```<button>``` element has three main features:

- Buttons can receive focus.
- Buttons can be operated conventionally by the keyboard once focused.
- Buttons are announced as “button” by screen readers.

Making websites usable by keyboard is part of WCAG 2.0 (the Web Content Accessibility Guidelines, published by the Web Accessibility Initiative).

On the Bundlin website, all focus pseudo-classes are disabled. Therefore, the website is not accessible for keyboard users. When there is no focus on the selected item, the user has no clue about his position on the website. For screen readers, it’s necessary to make proper use of ```<button>```’s and ```<a>```’s elements for clickable areas or links. So creating a ```<div>``` with an event listener in Javascript is not accessible for tabbing or screen readers. 

So ```* { outline: none; }``` is not possible in today’s web development. If you don’t like the blue outline, create a new style, don’t disable it. 

```
<div class="playbutton bln-button bln-button-invert bln-button-play" ng-click="playVideo()" ng-show="!playvideo" ng-class="{'bln-button-played': video_played}">
	<span class="bln-icon bln-icon-play-button"></span>
    What's Bundlin all about?
</div>
```

```
<button class="playbutton bln-button bln-button-invert bln-button-play bln-button-played" ng-click="playVideo()" ng-show="!playvideo" ng-class="{'bln-button-played': video_played}">
    <span class="bln-icon bln-icon-play-button"></span>
    What's Bundlin all about?
</button>
```

The next thing I noticed was the video on the homepage, which is not visible, till the user clicks the button above. But when the user is using the keyboard and tabs through the website, even when the container is not folded you tab through all the buttons inside the Vimeo video. The solution is a display: none; and display: block; when the button is clicked. 

```
.bln-section-video-intro {
    display: none;
}

.bln-section-video-intro.active {
    display: block;
}
```

Source: Apps for all By Heydon Pickering

###Expanding the clickable area 

On mobile phones or tablets, where clicks become touch, it's sometimes hard to target little links with your finger. Mostly because it was primarily designed for a desktop use. Jacob Rossi, an Internet Explorer 10 engineer who worked on the Microsoft's pointer API, said at the 2013 W3Conf that every touch oriented elements should be at least 40x40 pixels wide, to ensure the best user experience.

```
.bln-button:after {
    content: '';
    position: absolute;
    top: -10px; bottom: -10px; 
    left: -10px; right: -10px; 
}
```

Source: [Front-back](http://front-back.com/expand-clickable-areas-for-a-better-touch-experience)










