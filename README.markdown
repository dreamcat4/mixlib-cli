## Mixlib::CLI

Uh - Its all really totally different now! But in a good way :)

### Dreamcat4's Mods

See `examples/cli.rb`. Download and run `examples/cli.rb --help`

New features (not completely polished, but should work)

* Arguments ie subcommands, etc. Declare them with the new `argument` method (complements the existing `option` method).
  
  Arguments are just added / stripped their leading `--` and sent to optionparser as if they were `--options`. This means that all the features of `--options` are available for `arguments` too.

* Collision detection for arguments and options (as they cannot have the same name).

* OrderedHash to preserve the order both options and arguments. Helps keep the `--help` page readable / organised.

* No more `bool => true`. Unnecessary. Use optionparser's `--[no-]option` instead.

* Either print the compact `--help`, or a spaced out version (easier to read). Just set `spaced_summary true`.

* Headers, footers, and can set the column width with `summary-width <integer>`

* An indent option to indent those more minor flags for the `--help` page.

* Expose / use more of optionparser's native syntax.
  
  We support the OptionParser examples at -  [http://ruby-doc.org/core/classes/OptionParser.html](http://ruby-doc.org/core/classes/OptionParser.html) 

* Keywords, which is optionparser's keywords completion. Declare either an Array or a Hash of the keywords.
  Eg 
  
  `:keywords => [:one, :two, :three]`,
  
  `:keywords => { :one => "value one", :two => "value two", :three => "value three"}`.

* Can set the `:type` (sent to optionparser) for `Array, Float, Integer, Time, Date, etc and so on...`
  
  See - [http://blog.segment7.net/articles/2008/01/05/optionparser-argument-casting](http://blog.segment7.net/articles/2008/01/05/optionparser-argument-casting)

* Single or Multi-line descriptions. Just set `:description => ["line 1", "line 2", "etc"]`

* Single or Multi-line code examples. `:example => ["subcommand --flag", "sudo cmd subcommand --flags"]`

* `:required` is removed (deprecated) as its of very limited use.

* New `:requires => Proc` key, allows you to define arbitrarily complex options interdependancies.
  
  For example, `:requires => Proc.new { |c| ( c[:user] ^ c[:boot] ) ? true : "--user|--boot" }`
  
  ^ That means XOR either --user --boot (not both), then return true (passes test), 
  
  else print the error msg `"option requires --user|--boot"`

* Or simply `:requires => :other_option`, or `:requires => [:other_option1, :other_option2, :etc]`

* A new accessor attribute `filtered_argv` (perhaps should be renamed to `remaining_argv`) - which is the unparsed options left over that didnt match. Useful to access and re-parse later on depending on the subcommand.


### Whats unfinised in Dreamcat's mods

* Should wrap the optionparser.parse! in a `begin - resuce - end`. And intercept the `OptionParser::Exceptions` to raise more human-readable error messages.

* When OptionParser raises an Exception, it always reports the switchified arguments. Eg a sumcommand `edit` will be printed back as `--edit`. This is because OptionParser can only process options and we re-write arguments as options. We should catch the Exception.message string re-write the arguments back again.

* The Indent feature (setting `:indent => true`) will ever indent an option by 2 spaces. It would be good to give a more flexible, nestable indent, such as `:indent => 4` and `:indent => "some_indent_string"`

* Generally make the new code easier to read, understand and maintain. Lots of explanatory comments could / should be added to the new code. Perhaps a re-think for the way its been written as has grown in size.


### New in 1.2.0

We no longer destructively manipulate ARGV.

Have fun!
