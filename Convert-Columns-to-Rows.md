Most responsive email layouts feature single fluid columns, whose contents adjust to the width of the device. This experience may be underwhelming for desktop users who have more real estate for content. 

To illustrate how to create a more complex and interesting layout, for example columns that transform into rows for smaller screens, we'll walk through the code in [`3columns-to-rows.html`](blob/master/templates/3columns-to-rows.html), which works across all major email clients, *including* Outlook.

## Columns Container Table

In your desktop version, to create columns as you would normally would with `<table>`s _not_ `<div>`s as some recommend.
An example columns container looks like this.

````
	<table border="0" cellpadding="0" cellspacing="0" class="columns-container">
		<tr>
			<td class="force-col" valign="top">
				Column 1				
			</td>
			<td class="force-col" valign="top">
				Column 2
			</td>
		</tr>
	</table>
````

To manipulate the columns into rows, you need the following styles in your `<head>`

````
	@media only screen and (max-width : 600px) {
		td[class="force-col"] {
			display: block;
		}
   }
````

I choose a breakpoint of `600px` instead of `480px` to catch small tablets in addition to smart phones. 

This example demonstrates converting 2 columns into rows, but the strategy is the same for more columns. For each additional column, add another `<td>` with the `force-col` class. The mobile styles require no additional editing.

## Manipulating Content Layout between desktop and mobile

Now let's improve our layout a bit and add content to our columns. Here we have a simple layout that flows like this:  

![Example Column Content Layout](http://internations.github.com/antwort/images/content-column.png "Example Column Content Layout")

which basically consists of the following:  

* image, note class of `col-3-image`  
* headline  
* short copy


Look at [`3columns-to-rows.html`](blob/master/templates/3columns-to-rows.html) and see we wrap out content in a table like so:

````
	<table border="0" cellspacing="0" cellpadding="0" width="175" align="left" style="padding-right: 20px;" class="col-3">
		<tr>
			<td align="left" valign="top" style="font-size:13px; line-height: 18px; font-family: Arial, sans-serif;">
				
				<!-- finally column content here -->
				<img src="images/grey-175x130.png" alt="Image Caption" border="0" hspace="0" vspace="0" style="vertical-align:top; margin-bottom:12px;" class="col-3-img">
				<a href="#" style="color:#2469A0; font-weight:bold">Herman Melville</a><br>
				It's worse than being in the whirled woods, the last day of the year! Who'd go climbing after chestnuts now? But there they go, all cursing, and here I don't.<br>
				
			</td>
		</tr>
	</table>
````

We need this extra wrapper table because in our mobile version we want to float/align the image to the right of the text, like this:

![Column Content Transformed to Row Layout](http://internations.github.com/antwort/images/content-row.png "Column Content Transformed to Row Layout")

Whereas in normal HTML you need only apply `clear: both;` to an element, that does not work for some versions Outlook. The bulletproof way to clear floated images is to wrap them in a `<table>`, above with a class of `col-3`.

And then we manipulate the layout with the following CSS *inside your media query*:

````
        table[class="col-3"] {
            /* unset <table> align="left/right" */
            float: none !important;
            width: 100% !important;

            /* change left/right padding and margins to top/bottom ones */
            margin-bottom: 12px;
            padding-bottom: 12px;
            padding-right: 0 !important;
            border-bottom: 1px solid #eee;
        }

        /* align images right and shrink them a bit */
        img[class="col-3-img"] {
            float: right;
            margin-left: 6px;
            max-width: 130px;
        }

````

