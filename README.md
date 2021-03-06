A simple utility that lets you use LINQ syntax to query HTML.

## Samples

    const string html = @"
    <html>
        <head>
            <TITLE>The Title</TITLE>
        </head>
        <body bgcolor='#ffffff'>
            <H1 id='hello' style='greeting' tabkey='1'>Hello</H1>
            <H2 id='world' style='greeting' tabkey='2'>World</H2>
        </body>
    </html>";

    var parsed = HTMLParser.Parse(html);

    parsed
        .Head
        .Content
        .ShouldBeEqualTo("The Title");

    parsed
        .ChildTags
        .Count()
        .ShouldBeEqualTo(2); // head, body

    parsed
        .DescendantTags
        .Count(x => x.Parent.TypeEqualsIgnoreCase("HTML"))
        .ShouldBeEqualTo(2); // head, body

    parsed
        .DescendantTags
        .OfType("title")
        .Any()
        .ShouldBeFalse();

    parsed
        .DescendantTags
        .OfType("title").IgnoreCase()
        .Any()
        .ShouldBeTrue();

    parsed
        .DescendantTags
        .OfType("title").IgnoreCase()
        .First()
        .Parent
        .TypeEqualsIgnoreCase("HEAD")
        .ShouldBeTrue();

    parsed
        .DescendantTags
        .WithAttributeNamed("style")
        .Count()
        .ShouldBeEqualTo(2); 

    parsed
        .DescendantTags
        .WithAttributeNamed("id")
        .HavingValue("world")
        .First()
        .Content
        .ShouldBeEqualTo("World");

mapping

    public class Text
    {
        public string Id { get; set; }
        public int TabKey { get; set; } 
    }
    
    var text = new Text();
    parsed
        .DescendantTags
        .OfType("H1")
        .First()
        .MapTo(text);

    text.Id.ShouldBeEqualTo("hello");
    text.TabKey.ShouldBeEqualTo(1);
        
## License        

note: the .Should... syntax comes from the [FluentAssert][FluentAssert] library.

[MIT License][mitlicense]

This project is part of [MVBA Law Commons][mvbalawcommons].

[mvbalawcommons]: http://code.google.com/p/mvbalaw-commons/
[FluentAssert]: http://github.com/mvba/FluentAssert/
[mitlicense]: http://www.opensource.org/licenses/mit-license.php   
