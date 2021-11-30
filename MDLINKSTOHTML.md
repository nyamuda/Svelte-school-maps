# Convert MD links to HTML using Javascript

This is a sample function for converting Markdown-formatted links to HTML.

    function addLinks(string) {
    let elements = string.match(/\[.*?\)/g)
    if (elements != null && elements.length > 0) {
        for (let el of elements) {
        let txt = el.match(/\[(.*?)\]/)[1]
        let url = el.match(/\((.*?)\)/)[1]
        string = string.replace(
            el,
            '<a href="' + url + '" target="_blank">' + txt + '</a>'
        )
        }
    }
    return string
    }
