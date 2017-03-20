# spaceplan-optimizer

Hacky coding excerise I decided to spend an hour on, which tells you the best purchases for idle game [Spaceplan](http://jhollands.co.uk/spaceplan/). It appends a "time till return on investment" to the Thing Maker information window, refreshed on click.

![Imgur](http://i.imgur.com/fEBtxyh.gif)

The game's HTML is a bit... sub-optimal, so I expect this script to break at some point.

The `##### s` value shows you the seconds until you make all your power back from buying a thing, always go for the one with the lowest number.

**Doesn't work early game!** Although you don't really need is as much early game anyways.

Script will usually show `NaN` when there's an issue

## Usage

Open inspect element after the page has fully loaded, then open the console.

In the console, paste in the following script:

```javascript
function sigFigs(n, sig) {
  var mult = Math.pow(10, sig - Math.floor(Math.log(n) / Math.LN10) - 1);
  return Math.round(n * mult / mult);
}

function update_stats(){
  $('#manufacture__container .manufacture__item, #manufacture__container .manufacture__item--locked').each(function() {
    cost = $(this).find("#cost").text().replace(/\,/g,'');
    income = $(this).find("#powerGain").text().replace(/\,/g,'').replace('w/sec','');
    ROI = sigFigs(cost / income, 2);
    $(this).find(".ROI").text(` ${ROI} secs`);
  });
}

function add_ROI(){
  $('#manufacture__container .manufacture__item #costLine, #manufacture__container manufacture__item--locked #costLine').each(function() {
    if ($(this).text().includes('secs') == false) {
      $(this).append("<span class='ROI'> working...</span>");
    }
  });
}

function work(){
  add_ROI();
  update_stats();
}

function setup() {
  work();
  $(document).click(function() {work();});
}

function load_jQuery(callback) {
  if ( window.jQuery )
    return callback();

  var jQuery_element_ID = 'spaceplan-optimizer-jquery';
  
  if ( !document.getElementById(jQuery_element_ID) ) {
    var script = document.createElement('script');
    script.type = 'text/javascript';
    script.src = 'https://ajax.googleapis.com/ajax/libs/jquery/2.2.0/jquery.min.js';
    script.id = jQuery_element_ID;

    document.getElementsByTagName('head')[0].appendChild(script);
  }

  window.setTimeout(function() { load_jQuery(callback); }, 100);
}

load_jQuery(setup);
```
