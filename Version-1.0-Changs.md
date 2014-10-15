Antwort Version 1.0 is a complete rewrite of the code. The change was prompted largely by changes made to the Android Email clients effective with the Android 4.3 release in mid-2013.

### Increased Android support

While many of us love our iOS devices, the unfortunate truth is that **Android has 80% market share vs. iOS's ca. 15%** [(source: Business Insider)](http://www.businessinsider.com/iphone-v-android-market-share-2014-5). That being said, it is possible that you have more opens in iOS than in Android or other clients, as per email client market share stats from [Litmus](http://emailclientmarketshare.com/). Your numbers may vary drastically depending on your audience.

Despite all these uncertainties, Antwort aims to be as *bullet-proof-as-possible*, meaning it should be as least broken as possible.

### Android suddenly broke

But suddenly in mid-2013 Antwort v0 was **very broken** on Android devices, example:

* columns were no longer forced into rows
* columns rendered erratically, sometimes losing width properties altogether.

<table>
  <tr>
    <td>
      <img src="http://internations.github.io/antwort/images/guide/v0-2col-tds.png" alt="Very broken: Erratic column widths" width="300">
    </td>
    <td>
      <img src="http://internations.github.io/antwort/images/guide/v0-3col-android-email.png" alt="Broken: horizontal scrolling" width="300">
    </td>
  </tr>
  <tr>
    <td align="center">
      Very broken: Erratic column widths
    </td>
    <td align="center">
      Broken: horizontal scrolling
    </td>
  </tr>
</table>

Many were surprised by Android's changes:

- [Becs Rivett - Is responsive email dead for Android?](http://becsrivett.com/responsive-email-dead-android/) (18 Feb 2014)
- <a href="http://blog.cergis.com/posts/18/Consequences+of+the+Android+4.3+update+with+responsive+emails+(Part+1)">Cergis Blog - Consequences of the android 4.3 update with responsive emails</a> (6 Jan 2014)
- [Litmus community discussions: Samsung vs. Android email client](https://litmus.com/community/discussions/38-samsung-vs-android-email-client) (19 Feb 2014)

#### How Android broke

In Android 4.3+ `<td>`s always render as table cells. That means applying `display: block !important;` and setting their width to `100%` have no effect and will **not** transform our columns into rows.

We use table cells instead of floats to support Microsoft Outlook. Outlook does not support some HTML and CSS features because it [uses Microsoft Word to render emails](http://support.microsoft.com/kb/933793).

#### Simulating Floats with Tables

A `<table align="left">` behaves like a floated element and all email clients support tables. So let's use used multiple aligned tables to be our columns. To make sure Outlook aligns our columns correctly, we need a wrapper table, which we'll serve only to Outlook using the `[if mso]` conditional:

```html
<!--[if mso]>
<table border="0" cellpadding="0" cellspacing="0" width="100%">
  <tr><td width="50%" valign="top"><![endif]-->

    <table width="264" border="0" cellpadding="0" cellspacing="0" align="left" class="force-row">
      <tr>
        <td class="col" valign="top">
          <strong>Herman Melville</strong>
          <br><br>
          It's worse than being in the whirled woods, the last day of the year! Who'd go climbing after chestnuts now? But there they go, all cursing, and here I don't.
          <br><br>
        </td>
      </tr>
    </table>

    <!--[if mso]></td><td width="50%" valign="top"><![endif]-->

    <table width="264" border="0" cellpadding="0" cellspacing="0" align="right" class="force-row">
      <tr>
        <td class="col" valign="top">
        <strong>I am Ishmael</strong>
        <br><br>
        White squalls? white whale, shirr! shirr! Here have I heard all their chat just now, and the white whale—shirr! shirr!—but spoken of once! and&hellip;
        <br><br>
        </td>
      </tr>
    </table>

<!--[if mso]></td></tr></table><![endif]-->
```

You need to set a *fixed pixel width* for the tables. Here we have `264px`, which in this case is `(600-3*24)/2` or our container minus gutters divided by number of columns.

#### Force column tables into rows

In the example above, our tables have fixed width of `264px` a `.force-row` class. To force them into rows on mobile we use the following styles:


```css
@media screen and (max-width: 599px) {
  table[class="force-row"] {
    width: 100% !important;
    max-width: 100% !important;
  }
}
```

Using this strategy, Antwort v1 renders like so on Android:

<table>
  <tr>
    <td>
      <img src="http://internations.github.io/antwort/images/guide/v1-3cols-andoird-email.png" alt="Android Email app properly transforms columns to rows" width="300">
    </td>
    <td>
      <img src="http://internations.github.io/antwort/images/guide/v1-3col-anroid-gmail.png" alt="Android Gmail makes it own rows" width="300">
    </td>
  </tr>
  <tr>
    <td align="center">
       <strong>Android Email app</strong><br>properly transforms columns to rows
    </td>
    <td align="center">
      <strong>Android Gmail app</strong><br>makes it own rows by fudging our widths
    </td>
  </tr>
</table>

Depending on your design, this may not be desired behavior, esp. on Android Gmail app. Check your audience's email client share statistics before deciding if this strategy works for you.
