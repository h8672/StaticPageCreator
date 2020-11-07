# Oma Static Page

Compiles static webpage from multiple parts.

Example to create simple static page using target as content

## content/target.static contains structure for static page...

```csharp

{{!-- Single line comment example --}}
{{!--}}
	Multiline comment example
	Ignores {{commands}} and they can be commented here.
	None of these texts or {{commands}} will be saved to the results though...
{{--}}

{{!--}}
	These comments are not added to the result file...
	Comments all lines until it finds end of the comment...
{{--}}

{{!--}} Changing default selectors for current process. {{--}}
{{process --selectors "{" "}"}}
{!--} Comments are also changed according to the selectors. {--}

{process --default-selectors}
{{!--}}
	If there happens to be some code that is hard to use with current selectors.
	You can switch between selectors at anytime.
{{--}}

{{!--}}
	Start to process of other static file from this file.
	It is a new instance of this. Used to build all sites from single file.
{{--}}
{{process --process other.static}}

{{!--}}
	Get keywords from a file, but nothing else happens.
	Keys are added to current process pool.
	I think I should add support for some database format here...
	Like: Attribute And lots of text after attribute. Saved until new line to attribute.
{{--}}
{{process --keywords content/data/default_keywords.txt}}

{{!--}}
	This instance own keywords.
	If keyword is found from included file, the value is written there.
	keyword/kw keyword_name keyword_value_as_a_string_until_enf_of_line(EOL)
{{--}}
{{keyword title Default title that should be overwritten...}}
{{keyword year 2020}}
{{kw and &#38;}}
{{kw copyright_mark &#169;}}

{{!--
	Base text for each new page added in this file.
	Base start and end.
	Base content tells where the new page data is added.
	Not needed if there's only a single page parsed in the file.
--}}
{{base start}}
<!DOCTYPE html>
<html>
	{{base content}}
</html>
{{base end}}

{{!--
	Or out source it to make this file shorter...
	If you don't want to retype base every time.
--}}
{{process --base content/block/base.html}}

{{!--
	Start of the new page and always required.
	Makes the result cleaner with no random new lines.
	Marks the start of the instance for the parser.
--}}
{{new}}

{{!-- Page only keyword. Will be used before global keywords. --}}
{{keyword title New Webpage using my own Static Webpage creator!}}

{{!-- Add this page instance keywords in a single file. --}}
{{process --keywords content/data/main_page_keywords.txt}}

<head>
	{{!-- Add header.html content here. --}}
	{{include content/block/header.html}}

	<meta charset="utf-8">
	<link rel="stylesheet" href="/css/widepage_style.css">
	<script src="target_page.js" charset="utf-8"></script>
</head>

<body>
	{{!-- Adds logo and navigation menu... --}}
	{{include content/block/top.html}}

	{{!-- Adds sidebar --}}
	{{include content/block/sidebar.html}}

	{{!--Adds preview content--}}
	{{include content/block/target_content.html}}

	{{!-- Method was added in some js file that was included in header.html... --}}
	<script>LoadPageDetailsOnStart();</script>
</body>

<footer>
	{{!-- Adds footer content --}}
	{{include content/block/footer.html}}
</footer>

{{!-- End of the page process and save location for the results. --}}
{{save result/target.html}}

{{!--}}
	Start process of the next page from outsourced file.
	Uses current keywords and base to process the next page.
{{--}}
{{process --new content/next_target.static result/next_target.html}}

```

## content/block/footer.html contains

```

watermark
{{copyright_mark}} {{year}} All rights reserved.

```

When this is run, it stacks all content.
Then it goes through and looks for some {keywords} in the content of each page.

Perhaps later add syntax for filetype that tells things in
.class #id div format...
But does this really need it?
I could just make '{' and '}' use different characters for the project...
Perhaps # and end-of-line character...

I could make or use additional processor to process .class #id div formats...
