A collection of metadata associated with the collection of the [Minneapolis Institute of Arts](http://artsmia.org/).

The [MIA's mission](http://new.artsmia.org/about/museum-info/mission-and-history/) is to enrich the community

> by collecting, preserving, and making accessible outstanding works of art from the world’s diverse cultures.  

To increase access to the MIA's outstanding art, we've published metadata for ~90,000 artworks as `JSON` under a [CC0](#cc0-by) license.

## Why

This is the same data that we publish on our [collections
website](https://collections.artsmia.org). Having everything on its own
page is a great way to browse, but taken as a whole it's much easier to
see the overall shape of the data. For example, if someone wants
to know how many artists have work at the MIA,

```bash
$ find objects -name "*.json" | xargs cat \
	| jq -r '.artist' \
	| sort | uniq -ci | sort -nr
```

is a simple program [that knows](https://gist.github.com/kjell/34439abc72d99d81f1e2). How many of those artists have the same name as you? Save the output of the above command to a file called `artsmia-artists.txt`, then `cat artsmia-artists.txt | grep -i <your name here> | wc -l` will tell.

Or maybe you want to use [markov
chains](https://en.wikipedia.org/wiki/Markov_chain) to make fake
descriptions of objects in official museum-speak…

```bash
$ find objects -name "*.json" | xargs cat \
	| jq -r '.description, .title, .text' \
	| grep -v '^$' \
	> descriptions.txt
$ dadadodo -c 1 descriptions.txt
Black lines of concentric diamonds (beard architectural African symbols
in the foot; body and black plum blossom petals).
```

(For these two examples, you'll need [jq](http://stedolan.github.io/jq/)
and [dadadodo](http://www.jwz.org/dadadodo/), along with a sane command
line environment.)

Having this raw data in an accessible format opens the doors to
experimentation that isn't possible with our collections website alone.

## Use

The MIA identifies objects curatorially by *accession number*. Computertorially, we use numeric `object id`s beacuse they're easier to deal with.

Each record lives at `objects/$bucket/$id.json`, where 'bucket' is `object id / 1000`. So `0/` holds records 0 through 999; `1/` holds 1000-1999; etc. Here's what [`objects/0/17.json`][] looks like:

```json
{
  "accession_number": "13.59",
  "artist": "Walter Shirlaw",
  "continent": "North America",
  "country": "United States",
  "creditline": "Gift of Mrs. Florence M. Shirlaw",
  "culture": null,
  "dated": "19th century",
  "description": "",
  "dimension": "3 1/4 x 7 1/2 in. (8.26 x 19.05 cm)",
  "id": "http://api.artsmia.org/objects/17",
  "image": "valid",
  "image_copyright": "",
  "image_height": 2263,
  "image_width": 3593,
  "life_date": "American, 1838 - 1909",
  "marks": "Signature [Cheyenne. W. Shirlaw]",
  "medium": "Graphite",
  "nationality": "American",
  "provenance": "",
  "restricted": 0,
  "role": "Artist",
  "room": "Not on View",
  "style": "19th century",
  "text": "",
  "title": "Sketch made on Indian Reservation, Montana"
}
```

[`objects/0/17.json`]: https://github.com/artsmia/collection/blob/master/objects/0/17.json

## Images

**Images aren't included under the same license as this metadata**. Reference images are available under the MIA's [Image Access & Use](http://new.artsmia.org/visit/policies-guidelines/#image_access_and_use) policy.

There aren't images of every object in the collection. Of the objects
that have been photographed, some are restricted by copyright.

* `"image": "valid|invalid"`: is there an image available for this object?
* `"restricted": "0|1"`: is it usable? `0` says that an image is not "subject to any additional terms or restrictions", such as copyright or a special request from the artist to limit display of an object. `1` means the opposite.

Feel free to use unrestricted images for "limited non-commercial and educational purposes". When the copyright owner of an artwork is known, it will be in `"image_copyright": "…"`. [Commercial licensing is handled through Bridgeman Images](http://www.bridgemanimages.com/en-GB/collections/collection/minneapolis-institute-of-arts/).

Image thumbnails are accessible by their `object id` in three sizes: `http://api.artsmia.org/images/$id/{small,medium,large}.jpg`. Small images are `100px` on the long side, medium images `600px` and large images `800px`.

## **[CC0][] [(+BY)][]**

The Creative Commons Zero (**[CC0][]**) Public Domain Dedication allows this data to be reused free of restrictions. **[(+BY)][]** suggests "*implied* or *ethical* attribution".

[CC0]: http://creativecommons.org/publicdomain/zero/1.0/
[(+BY)]: http://dp.la/info/2013/12/04/cc0-by/

Please use this data. When you do,

* Attribute the MIA and mention [`artsmia/collection`](https://github.com/artsmia/collection).
* [Pull request](https://help.github.com/articles/creating-a-pull-request) your changes, or [issue](https://github.com/artsmia/collection/issues) ideas and discussion.
* Obey the terms of this license and our image policy.
* Enjoy!

## See also

* [What's on view at the MIA dot `csv`](https://github.com/miabot/galleries.csv)
* [Europeana](http://www.europeana.eu/)
* [Cooper-Hewitt](http://labs.cooperhewitt.org/2012/releasing-collection-github/) ([cooperhewitt/collection](https://github.com/cooperhewitt/collection/))
* [DPLA](http://dp.la/info/2013/12/04/cc0-by/)
* [Tate](http://www.tate.org.uk/context-comment/blogs/archives-access-project-open-data-brings-beauty-and-insight) ([tategallery/collection](https://github.com/tategallery/collection))
* [Museum APIs](http://museum-api.pbworks.com/w/page/21933420/Museum%C2%A0APIs) and
  [open data](http://www.museum-id.com/idea-detail.asp?id=387)

