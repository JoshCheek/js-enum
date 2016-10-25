Hibrow-LoDash
=============

LoDash gives you some nice "enumerable" helpers (some people would call these "iterable").
These methods help you work with collections like arrays and objects.
But it's important to understand what abstractions actually do,
so lets figure out how to do those things without LoDash.


Setup
-----

Install `jq`

```sh
$ brew install jq
```

Some vague semblence of a plan
------------------------------

* 30 min lets play with these functions
  `node -p 'Reflect.ownKeys(Array.prototype)'`
* 90 min lets try using them on API data!

Examples (30 min)
-----------------

These are the potentially interesting methods:

```javascript
> Reflect.ownKeys(Array.prototype)
[ 'length', 'constructor', 'toString', 'toLocaleString', 'join', 'pop', 'push', 'reverse', 'shift', 'unshift',
  'slice', 'splice', 'sort', 'filter', 'forEach', 'some', 'every', 'map', 'indexOf', 'lastIndexOf', 'reduce',
  'reduceRight', 'copyWithin', 'find', 'findIndex', 'fill', 'includes', 'entries', 'keys', 'concat',
  Symbol(Symbol.unscopables), Symbol(Symbol.iterator) ]
```

* Predict the result of each of these
* Try them to see if you're right!
* Explain them in one sentence to the human adjacent to you
* Say in one sentence when you would choose to use them

```javascript
["a", "b", "c"].length     // what will this be?

["a", "b", "c"].join("-")  // what will this be?

let ary = ["a", "b", "c"]
ary.pop()                  // what will this be?
ary                        // what will this be?

ary = ["a", "b", "c"]
ary.push("d")
ary                        // what will this be?

ary = ["a", "b", "c"]
ary.concat("d")            // what will this be?
ary                        // what will this be?

ary = ["a", "b", "c", "d", "e"]
ary.slice(0)               // what will this be?
ary.slice(1)               // what will this be?
ary.slice(2)               // what will this be?
ary.slice(3)               // what will this be?
ary.slice(1,2)             // what will this be?
ary.slice(1,3)             // what will this be?
ary.slice(2,4)             // what will this be?

ary = ["a", "b", "c", "d", "e"]
ary.map((char) => char.toUpperCase()) // what will this be?
ary.map((char) => char + char)        // what will this be?

let chars = ""
["a", "b", "c", "d", "e"].forEach((char) => chars = chars + char)
chars // what will this be?

chars = ""
["a", "b", "c", "d", "e"].forEach((char) => chars = char + chars)
chars // what will this be?

ary = ["a", "b", "c"]
for(let i of ary) console.log(i)      // what will be printed?

let name
let obj = {"a": 1, "b": 2, "c": 3}
for(name in obj) console.log(name)      // what will be printed?
for(name in obj) console.log(obj[name]) // what will be printed?

ary = ["a", "b", "c"]
for(name in obj) console.log(name)      // what will be printed?
for(name in obj) console.log(obj[name]) // what will be printed?

["a", "bc", "d", "ef"].filter((str) => str.length === 2) // what will be printed?
["a", "bc", "d", "ef"].filter((str) => str.length === 1) // what will be printed?
"a bc d ef".split(" ")                                   // what will be printed?
"a bc d ef".split(" ").filter((str) => str.length === 2) // what will be printed?


let users = [
  {name: "Josh",  age: 33},
  {name: "Jan",   age: 38},
  {name: "Carla", age: 82},
  {name: "Mei",   age: 14},
]

// what will each of these be?
users.map((user) => user.name)
users.map((user) => user.name).map((name) => name.length)
users.map((user) => user.name).filter((name) => name[0] === "J").map((name) => name.length)
```

How to play with API data
-------------------------

```sh
$ curl 'http://api.zippopotam.us/us/80202'
{"post code": "80202", "country": "United States", "country abbreviation": "US", "places": [{"place name": "Denver", "longitude": "-104.9946", "state": "Colorado", "state abbreviation": "CO", "latitude": "39.7491"}]}⏎                                       09:33 AM   ~/code/jsl/hibrow-lodash   master


$ curl 'http://api.zippopotam.us/us/80202' | jq .
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   216  100   216    0     0    770      0 --:--:-- --:--:-- --:--:--   768
{
  "post code": "80202",
  "country": "United States",
  "country abbreviation": "US",
  "places": [
    {
      "place name": "Denver",
      "longitude": "-104.9946",
      "state": "Colorado",
      "state abbreviation": "CO",
      "latitude": "39.7491"
    }
  ]
}


$ curl 'http://api.zippopotam.us/us/80202' > denver-info.json
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   216  100   216    0     0   2688      0 --:--:-- --:--:-- --:--:--  2700


$ cat denver-info.json
{"post code": "80202", "country": "United States", "country abbreviation": "US", "places": [{"place name": "Denver", "longitude": "-104.9946", "state": "Colorado", "state abbreviation": "CO", "latitude": "39.7491"}]}


$ node -p 'require("./denver-info.json")'
{ 'post code': '80202',
  country: 'United States',
  'country abbreviation': 'US',
  places:
   [ { 'place name': 'Denver',
       longitude: '-104.9946',
       state: 'Colorado',
       'state abbreviation': 'CO',
       latitude: '39.7491' } ] }


$ node -p 'require("./denver-info.json").places[0].latitude'
39.7491
```


Try some shit out
-----------------

* What are the names of the runtime dependencies of Ruby on Rails?

  ```sh
  $ curl https://rubygems.org/api/v1/gems/rails.json
  $ curl https://rubygems.org/api/v1/gems/rails.json | jq .
  $ curl https://rubygems.org/api/v1/gems/rails.json > rails.json
  ```
* `https://api.github.com/users/{YOURNAME}/events/public`
  * filter github events and grab all the commits
  * Find the set of events you've done, this is stored on the type (don't allow duplicates)
  * Get the set of events you've done and their repo name (output should match this)

    ```sh
    $ cat your-githubz.json | jq 'map({name: .repo.name, type: .type})'
    ```
* `https://api.github.com/repos/turingschool/front-end-curriculum/commits`
  * The names of each committer to curiculum (the output should match this command)

    ```sh
    $ curl https://api.github.com/repos/turingschool/front-end-curriculum/commits | jq 'map(.commit.author.name)'
    ```
  * Same as above, but the names should be unique

    ```sh
    $ curl https://api.github.com/repos/turingschool/front-end-curriculum/commits | jq 'map(.commit.author.name)[]' | tr -d '"' | sort -u
    ```
  * The Avatar urls of the committers to your curriculum
  * Then put them on an html page (redirect the output to print to a file instead of the console)

    ```sh
    $ node -e 'console.log("<h1>hello</h1>")'
    <h1>hello</h1>

    $ node -e 'console.log("<h1>hello</h1>")' > index.html

    $ cat index.html
    <h1>hello</h1>
    ```
  * The set of unique words in commit messages
  * The set of unique words (with count) in commit messages (both case sensitive and insensitive)
  * The most frequently used word in a commit message
  * Same, but omit common words (the, a, in, etc)
  * Same, but the top 2 words
  * Same, but the top 10 words
  * For each of the top 10 most frequently used words in a commit message, they should be a key in an object, with a value that shows how many times they were used
  * Same, but instead of the count, map them to a list of words that rhyme with them
    You'll need to use this other API to do that: https://api.datamuse.com/words?rel_rhy=turing
  * Grand finale
    * Translate into an html file with their avatar, name, image, link to the commit
  * For each committer, their name is a key in an obj, the value is an array of the times that they committed (get it off the push event)
* other
  * How many commits did everyone on your team make in your last project? (base this off the curriculum url)
  * All the commit messages (ie like `git-log`)

    ```sh
    $ cat your-githubz.json | ruby -r json  -e 'puts JSON.parse($stdin.read).select { |e| e["type"] == "PushEvent" }.flat_map { |e| e["payload"]["commits"] }.map { |e| e["message"] }.to_json' |  jq .`
    ```

Things that humans want me to make sure you know
------------------------------------------------

* Keys are unique
* Arrow functions implicitly return
* duplicated data vs reference to the same object in memory
