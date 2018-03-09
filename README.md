# ready
A pattern to bootstrap any JavaScript on DOMContentLoaded.

[Live test](https://webreflection.github.io/ready/).


## the pattern
```html
<!-- add these 2 script on top of every page * -->
<!-- * these MUST be two separate scripts! -->
<script>this.Promise||document.write('<script src="//cdn.jsdelivr.net/npm/es6-promise/dist/es6-promise.auto.min.js"><\x2fscript>')</script>
<script>this.ready=new Promise(function($){document.addEventListener('DOMContentLoaded',$,{once:true})})</script>
<!-- so that every other script after can simply use ready.then(...) to bootstrap -->
<script>
// proof of concept @ https://webreflection.github.io/ready/
ready.then(function () {
  document.body.innerHTML = 'Yeah, the <a href="https://github.com/WebReflection/ready">ready</a> pattern works!';
});
</script>
```


## in a nutshell

  * all browsers with native [Promise](https://caniuse.com/#feat=promises) support won't ever use `document.write`
  * all browser that don't have native `Promise` won't ever have issues with `document.write`
  * the two different `<script>` tags [grant the correct execution order](http://webreflection.blogspot.com/2009/12/documentwriteshenanigans.html)
  * the `{once:true}` third argument ensures in modern browsers the listener is cleaned up ASAP and in older browsers it grants that [there is a third argument](https://github.com/thinkpixellab/PxLoader/issues/5)
  * the `this.ready=` is used, instead of a quirky `ready=`, to ensure if your project does some sort of linting/variables leaking validation it wouldn't bother that script.
  * every browser can now use a single global `ready` promise to bootstrap


## why

In September 2015 there was an attempt to [bring `document.ready`](https://github.com/whatwg/html/issues/127) to every browser.

Philosophy took over, so that in October 2016 there was a follow up
attempt to [bring `document.{parsed,contentLoaded,loaded} promises`](https://github.com/whatwg/html/pull/1936) to every browsers.

That resulted into pretty much **nothing for 3 years**, that's why.


## improvements ?

  * if you are targeting modern browsers you can fully remove the first script.
  * if in 2039 they'll ship a global `ready` you can refactor your code via `grep ready.then`
  * shortcut targeting modern browsers only (Edge, Chrome, Safari, Firefox, others)

```html
<script>this.ready=new Promise($=>addEventListener('DOMContentLoaded',$,{once:!0}))</script>
```

You read that correctly, the `DOMContentLoaded` event reaches the global `window` too.


### license ?

You can do whatever you want with this pattern, I haven't invented anything.

However, be sure the used Promise polyfill license is compatible with your project.

The proposed [es6-promise](https://github.com/stefanpenner/es6-promise) one uses a MIT License.
