# Whitespace in HTML&mdash;It Matters

Every so often I see text nodes in HTML where the author uses the ancient convention of putting 2 spaces between sentences. This looks nice on type-written documents and perhaps, too, in terminals, which also use a `monospace font`. But, in fact, your two spaces will not be visible to the user who is viewing it through an HTML browser. She will see only one space. Do I think this is such a big deal? Not at all, but I think it highlights an important property of whitespace in HTML that can sometimes affect the layout of your page.

All whitespace, between `<tags>` and `text nodes` will collapse into a neatly packed single space. Let's look at a few examples.

So I want to quote Lenore by Edgar Allen Poe in my blog

    <div class="poem">
    AH, broken is the golden bowl!
         The spirit flown forever!
     Let the bell toll! — A saintly soul
         Glides down the Stygian river!
             And let the burial rite be read —
                 The funeral song be sung —
             A dirge for the most lovely dead
                 That ever died so young!
                     And, Guy De Vere,
                     Hast thou no tear?
                         Weep now or nevermore!
                     See, on yon drear
                    And rigid bier,
                        Low lies thy love Lenore!
    </div>

Looks good in the code, but when I check it out in the browser, I see

> AH, broken is the golden bowl! The spirit flown forever! Let the bell toll! — A saintly soul Glides down the Stygian river! And let the burial rite be read — The funeral song be sung — A dirge for the most lovely dead That ever died so young! And, Guy De Vere, Hast thou no tear? Weep now or nevermore! See, on yon drear And rigid bier, Low lies thy love Lenore! 

What happened? Oh right, I neeed `<br />` tags so all the lines show up separately.

    <div class="poem">
    AH, broken is the golden bowl!<br />
         The spirit flown forever!<br />
     Let the bell toll! — A saintly soul<br />
         Glides down the Stygian river!<br />
             And let the burial rite be read —<br />
                 The funeral song be sung —<br />
             A dirge for the most lovely dead<br />
                 That ever died so young!<br />
                     And, Guy De Vere,<br />
                     Hast thou no tear?<br />
                         Weep now or nevermore!<br />
                     See, on yon drear<br />
                    And rigid bier,<br />
                        Low lies thy love Lenore!<br />
    </div>

Which looks like

>AH, broken is the golden bowl!<br />
>     The spirit flown forever!<br />
> Let the bell toll! — A saintly soul<br />
>     Glides down the Stygian river!<br />
>         And let the burial rite be read —<br />
>             The funeral song be sung —<br />
>         A dirge for the most lovely dead<br />
>             That ever died so young!<br />
>                 And, Guy De Vere,<br />
>                 Hast thou no tear?<br />
>                     Weep now or nevermore!<br />
>                 See, on yon drear<br />
>                And rigid bier,<br />
>                    Low lies thy love Lenore!<br />

Well that's better, but where'd all the spacing go?

It collapsed. If you want 2 spaces to look like 2 spaces and a newline to really give you a new line, then you have to tell the browser that you're giving it **preformatted** text. Then every space, newline and tab will be rendered verbatim. You can do this using the `<pre>` tag or setting the css `white-space` property to `pre`.

    <pre class="poem">
    AH, broken is the golden bowl!
         The spirit flown forever!
     Let the bell toll! — A saintly soul
         Glides down the Stygian river!
             And let the burial rite be read —
                 The funeral song be sung —
             A dirge for the most lovely dead
                 That ever died so young!
                     And, Guy De Vere,
                     Hast thou no tear?
                         Weep now or nevermore!
                     See, on yon drear
                    And rigid bier,
                        Low lies thy love Lenore!
    </pre>

Voila!

<pre>
AH, broken is the golden bowl!
     The spirit flown forever!
 Let the bell toll! — A saintly soul
     Glides down the Stygian river!
         And let the burial rite be read —
             The funeral song be sung —
         A dirge for the most lovely dead
             That ever died so young!
                 And, Guy De Vere,
                 Hast thou no tear?
                     Weep now or nevermore!
                 See, on yon drear
                And rigid bier,
                    Low lies thy love Lenore!
</pre>

## I Don't Care About Poetry

Ok, fine. What are some real world situations where whitespace comes into play?

For starters, if you'd like to have two visible spaces between each sentence, you can't just put 2 spaces in the HTML. You have to use a non-breaking space with a regular space, like so: `See Jack.&nbsp; See Jack Run.`

> See Jack.&nbsp; See Jack Run.

Another time you'll see whitespace come into play is when you're trying to arrange for there to be no space between two elements because of how you'd like something to be styled. A good example of this is a syntax highlighter. Below is a the HTML used to represent a line of python on Github.

    <pre>
        <div class="line" id="LC1679">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="p">[</span><span class="s">"FFFF00"</span><span class="p">,</span> <span class="s">"Yellow"</span><span class="p">],</span></div>
    </pre>

Which looks like:
<pre>
<div class="line" id="LC1679">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="p">[</span><span class="s">"FFFF00"</span><span class="p">,</span> <span class="s">"Yellow"</span><span class="p">],</span></div>
</pre>

Notice that there are no line breaks between the spans. Here is what it would look like if you put line breaks between the spans. Note, I'm taking out the `<pre>`.

    <div class="line" id="LC1679">
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
        <span class="p">[</span>
        <span class="s">"FFFF00"</span>
        <span class="p">,</span>
        <span class="s">"Yellow"</span>
        <span class="p">],</span>
    </div>
    </pre>

<div style="font-family:monospace;">
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<span class="p">[</span>
<span class="s">"FFFF00"</span>
<span class="p">,</span>
<span class="s">"Yellow"</span>
<span class="p">],</span>
</div>

It's subtle but you can see there is a space after the open bracket, before the first comma, and before the end bracket.

This type of thing is only an issue when you want things to be right next to each other, which, in the case of my syntax highlighter, I do.
