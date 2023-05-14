# Context Dependant Probability Index From World Model

Probabilities alone only predict the likelihood of patterns, but usually misses important context that reduces the probabilities over time. This project aims for a more context dependent approach for assessing probabilities based on existing world models.

For example

Context: Concert
Genre: Classical
Instrument: Piano
Musician: Beethoven

~~~xml
<grammar context="Concert">
  <gender>The</gender>
  <noun>musician</noun>
  <adjective>emaculate</adjective>
  <conjunction>is</conjunction>
  <verb>playing</verb>
  <adjective>beautifully</adjective>
  <punctuation>!</punctuation>
</grammar>
~~~

On the other hand Naive Bayes only assessing the likelihood of a word or sentence, and generally this is less than adequate for realistic speech, as grammar is usually more deterministic. Instead you would use the grammar format above to create a realistic phrase, then use a baysian model to assess the likelihood of that sentence being said, and another process for determining why something was said despite the unlikelyness.

## Constructing A World Model Using ASCII
A chatbot sufficiently human like need a model of the inner world. Generally this isn't in entire chunk, but based on a vague notion of what objects are from prior visual context. However the problem is the command line can't process PNG or JPEG files, but must be procssed in the form of ASCII art.

Therefore the approach would be developing this world model based on abstract ASCII representation of a world model for objects important for that context.

![Example Image](http://localhost:3000/IMPUnit/ContextDependantProbabilities/raw/branch/main/_images/actual_images/bow.jpg)

Example code

~~~ruby
actual_images = Dir.entries("_images/actual_images").sort

# The first actual image starts at number two after factoring in directory names.
row = 2

# Overall size of directory including directory names.
total_size = actual_images.size.to_i

# Limits of iteration based on total size minus the starting item.
iteration = total_size - row

# The actual iteration through all total images.
iteration.times do
  system("jp2a _images/actual_images/#{actual_images[row]} > _textimages/#{actual_images[row]}.txt")

  open("_images/ascii/objects_seen.txt", "a") { |f|
    f.puts actual_images[row].tr ".", ""
  }

  row = row + 1
end
~~~

## Comparing Processed Image To Things One Knows
Out of the box, Naive Bayes doesn't have any kind of world model to compare things it sees. It needs to be familiar with real world objects and ideas that it may come across in unfamiliar situations:

If I see a dog, I know its a dog based on my prior experience with dogs, or having an idea of what a dog is. And this applies to person, places, or things.

~~~ruby
seen_objects  = File.readlines("_images/ascii/knew_objects.txt")
known_objects = File.readlines("_knowledgeset/ascii/already_seen.txt")

row = 0

comparison_limit = seen_objects.size.to_i

comparison_limit.times do
  object_seen = seen_objects[row]
  object_known = known_objects[row]

  if object_seen == object_known
    puts "This is a situation I am familiar with."
  else
    puts "This is an unfamiliar situation."
  end

  row = row + 1
end
~~~

## Grammar Based On Visual Context
The machine needs to be able to interprat this image in the form of useable data, generally in the form of XML, so that it can be used for parsing into a format that naive bayes can actually use.

~~~xml
<grammar context="Bedroom">
  <gender>The</gender>
  <noun>programmer</noun>
  <adjective>grungy</adjective>
  <conjunction>is</conjunction>
  <verb>typing</verb>
  <adjective>quickly.</adjective>
  <punctuation>!</punctuation>
</grammar>
~~~

Example code
~~~
# Reads in seen and known objects.
seen_objects  = File.readlines("_images/ascii/objects_seen/objects_seen.txt")
known_objects = File.readlines("_images/ascii/known_objects/known_objects.txt")

# By default the row is set to zero.
row = 0

# The total size of the loop is equal to the overall size of the list of seen objects.
comparison_limit = seen_objects.size.to_i

# Create an if then else loop that iterates through the situations in seen and knowns objects, and writes a DSXML script.
comparison_limit.times do
  object_seen = seen_objects[row]
  object_known = known_objects[row]

  # Writes the DSXML where situations are familiar.
  if object_seen == object_known
    puts "This is a situation I am familiar with."

    if    object_known ==       "bowjpg"
      open("_grammar/#{object_known}.xml", "w") { |f|
        f.puts '<grammar context="Bow">
  <gender>The</gender>
  <noun>hairbow</noun>
  <adjective>black</adjective>
  <conjunction>it is</conjunction>
  <verb>sitting</verb>
  <adverb>securely</adjective>
  <punctuation>.</punctuation>
</grammar>
        '
      }
    elsif object_known ==   "klompenjpg"
      open("_grammar/#{object_known}.xml", "w") { |f|
        f.puts '<grammar context="Klompen">
  <gender>The</gender>
  <noun>klompen</noun>
  <adjective>brown</adjective>
  <conjunction>I am</conjunction>
  <verb>wearing</verb>
  <adverb>comfortably</adjective>
  <punctuation>.</punctuation>
</grammar>
        '
      }
    elsif object_known ==     "shirtjpg"
      open("_grammar/#{object_known}.xml", "w") { |f|
        f.puts '<grammar context="Shirt">
  <gender>The</gender>
  <noun>shirt</noun>
  <adjective>black</adjective>
  <conjunction>I am</conjunction>
  <verb>wearing</verb>
  <adverb>comfortably</adjective>
  <punctuation>.</punctuation>
</grammar>
        '
      }
    elsif object_known ==    "shortsjpg"
      open("_grammar/#{object_known}.xml", "w") { |f|
        f.puts '<grammar context="Shorts">
  <gender>The</gender>
  <noun>shorts</noun>
  <adjective>black</adjective>
  <conjunction>I am</conjunction>
  <verb>wearing</verb>
  <adverb>formfittingly</adjective>
  <punctuation>.</punctuation>
</grammar>
        '
      }
    elsif object_known == "stockingsjpg"
      open("_grammar/#{object_known}.xml", "w") { |f|
        f.puts '<grammar context="Stockings">
  <gender>The</gender>
  <noun>stockings</noun>
  <adjective>red</adjective>
  <conjunction>I am</conjunction>
  <verb>wearing</verb>
  <adverb>securely</adjective>
  <punctuation>.</punctuation>
</grammar>
        '
      }
    else
      open("_grammar/#{object_known}.xml", "w") { |f|
        f.puts '<grammar context="Dressing">
  <gender>The</gender>
  <noun>girl</noun>
  <adjective>Dutch</adjective>
  <conjunction>I am</conjunction>
  <verb>dressing</verb>
  <adverb>formfittingly</adjective>
  <punctuation>.</punctuation>
</grammar>
        '
      }
    end
  else # Writes the DSXML where the situations are unfamiliar.
    open("_grammar/#{object_known}.xml", "w") { |f|
      f.puts '<grammar context="Unfamiliarty">
  <gender>The</gender>
  <noun>situation</noun>
  <adjective>unfamiliar</adjective>
  <conjunction>I am</conjunction>
  <verb>confusing</verb>
  <adverb>sleepily</adjective>
  <punctuation>.</punctuation>
</grammar>
        '
    }
  end

  row = row + 1
end
~~~

This is then used as the training example for training the Baysian algorithm.

## Pretraining A Model
The model needs to be pretrained on multiple domain specific XML patterns in order to operate as quickly as possible under pressure, and so it can run on consumer hardware. On smaller datasets you can get away with teaching the model every time you want to assess probabilities, but this can slow down runtime.

Such datasets are stored in _data/modeltype/dataset.nb, replaced modeltype with the name of your dataset, and dataset the name of your dataset plus the filename: .nb
