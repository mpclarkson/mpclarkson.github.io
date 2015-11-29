---
layout: post
title: Convert Dictionary Keys from snake_case to camelCase in Swift 2
comments: true
---

RESTful APIs often return `json` dictionaries with keys formatted using `snake_case`, particularly for back-ends build in Ruby or Python frameworks.
However, Swift typically uses the `camelCase` convention for object properties, which can make converting `json` responses to Swift objects a
little too verbose.

Never fear, with a few quick extensions you can make this conversion a breeze.

<!--more-->

Firstly, we need to write a `String` extension that removes underscores and capitalizes the character immediately following each underscore.
This is achieved as follows:

{% highlight swift %}

extension String {

    var underscoreToCamelCase: String {
        let items = self.componentsSeparatedByString("_")
        var camelCase = ""
        items.enumerate().forEach {
            camelCase += 0 == $0 ? $1 : $1.capitalizedString
        }
        return camelCase
    }
}

{% endhighlight %}

This `underscoreToCamelCase` variable splits any string in to an array of strings `[String]` using an underscore as a delimiter. We then
`enumerate` the string so we can track the index and each string in the array. The first element of the array should always be lowercase
(we are assuming the api always returns a lowercase first character here - i.e.`$0 (the index) == 0`) and each subsequent element is
capitalized.

Now we need a `Dictionary` extension that recursively checks each key and converts string keys to `camelCase`, as follows:

{% highlight swift %}

extension Dictionary {

    func keysToCamelCase() -> Dictionary {

        let keys = Array(self.keys)
        let values = Array(self.values)
        var dict: Dictionary = [:]

        keys.enumerate().forEach { (let index, var key) in

            var value = values[index]
            if let v = value as? Dictionary,
               vl = v.keysToCamelCase() as? Value {
                value = vl
            }

            if let k = key as? String,
            ky = k.underscoreToCamelCase as? Key {
                key = ky
            }

            dict[key] = value
        }

        return dict
    }
}

{% endhighlight %}

In this extension we convert an array of keys and an array of values for the dictionary (I'm assuming here that you have already converted the JSON
response to a dictionary). The keys array is enumerated, again so we can easily keep track of the `index` and the `key` without writing
verbose incrementers, such as `count++`.

For each key, we check if the corresponding value is a dictionary and also need conversion. If it is a dictionary, we apply the same `keysToCamelCase` function
recursively to the sub dictionary (which again does the same to each associated key and so on...). The resulting value, either the original value
or the new dictionary is then associated with the key.

For each key that is a string (which should be the case for all keys in a JSON response), we convert the key to camelCase using the
`keysToCamelCase` String extension.

And there you have it!

Code on!