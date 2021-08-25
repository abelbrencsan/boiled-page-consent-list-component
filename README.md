# Boiled Page consent list component

Consent list SCSS component for Boiled Page frontend framework. It is intended to create consent popups at the bottom of page.

## Install

Place `_consent-list.scss` file to `/assets/css/components` directory, and add its path to components block in `assets/css/app.scss` file. You will also need to add button component for properly working buttons, notice script to handle consent accept and dismiss events, cookie script to store user choice.

- Button component: <https://www.github.com/abelbrencsan/boiled-page-button-component>
- Notice script: <https://www.github.com/abelbrencsan/boiled-page-notice-script>
- Cookies script: <https://www.github.com/abelbrencsan/boiled-page-cookies-script>

## Usage

### Classes

Class name | Description | Example
---------- | ----------- | -------
`consent-list` | Applies a consent list. | `<ul class="consent-list"></ul>`
`consent-list-item` | Applies a consent list item. | `<li class="consent-list-item"></li>`
`consent-list-item-text` | Applies text inside a consent list item. | `<div class="consent-list-item-text"></div>`
`consent-list-item-text-button-list` | Applies a list of buttons inside consent list item text. Grid component is used for alignment. | `<ul class="consent-list-item-text-button-list grid"></ul>`

#### Examples

##### Example 1

The following example shows a cookie consent.

```html
<!-- Consent list -->
<ul class="consent-list">

  <!-- Cookie consent -->
  <li class="consent-list-item is-visible" data-consent="cookie-consent">
    <div class="container">
      <div class="consent-list-item-text">
        <p>This website uses cookies to ensure you get the best experience. Please read our <a href="#">privacy policy</a> for more details.</p>
        <ul class="consent-list-item-text-button-list grid grid--gutter grid--gutter--half grid--uniform grid--center">
          <li class="grid-col">
            <button class="button" data-consent-dismiss>Dismiss</button>
          </li>
          <li class="grid-col">
            <button class="button" data-consent-accept>Accept</button>
          </li>
        </ul>
      </div>
    </div>
  </li>
  
</ul>
```

Add `cookieConsent` and `isCookieConsentAccepted` properties to `app` object in `assets/js/app.js`.

```js
cookieConsent: null,
isCookieConsentAccepted: false
```

Place the following code inside `assets/js/app.js` to initalize element with `data-consent` attribute as a notice, store user choice in a cookie.

```js
var consentElem = document.querySelector('[data-consent="cookie-consent"]');
if (!Cookies.read('cookie-consent')) {
  consentElem.classList.add('is-visible');
  app.cookieConsent = new Notice({
    element: consentElem,
    dismissTrigger: consentElem.querySelector('[data-consent-dismiss]'),
    acceptTrigger: consentElem.querySelector('[data-consent-accept]'),
    dismissCallback: function() {
      Cookies.create('cookie-consent', 'off', 365);
    },
    acceptCallback: function() {
      Cookies.create('cookie-consent', 'on', 365);
    }
  });
  app.cookieConsent.init();
}
else {
  app.isCookieConsentAccepted = true;
}
```