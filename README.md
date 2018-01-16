# rubyencoder_error

Has both encoded and unencoded sample ruby on rails project.

"test_var_with_encoding" has the project with encoded files in it.

"test_var_without_encoding" has the same project without encoding.


Both projects are expected to work with Ruby 2.3.5.


Steps to reproduce the error:

	Command: cd test_var_without_encoding ## cd to un-encoded rails project

	Command: export PATH=./ruby/bin:$PATH ## Add ruby 2.3.5 to path

	Command: rake db:version 2> /dev/null ## This will call the content in config/environments/production.rb where production.rb is un-encoded

	Output : 

		 "defined?($rails_rake_task)"

		 "global-variable"

		 "$rails_rake_task"

		 true

		 Current version: 0


Observe the variable type of $rails_rake_task. It is "global-variable" if it is not encoded.

Now let's encode the project.


	Command: cd ../test_var_with_encoding ## cd to encoded rails project

	Command: /usr/local/rubyencoder-2.4.1.1/bin/rubyencoder --ruby 2.3 -b- -r -x 'ruby/*' -x 'vendor/*' -x 'rgloader/*' -x 'script/*.ps1' -x 'db/*.rb' '*.rb' ## Encode the project.

	Command: rake db:version 2> /dev/null ## This will call the content in config/environments/production.rb where production.rb is encoded.

	Output: 

	       "defined?($rails_rake_task)"

	       nil

	       "$rails_rake_task"

	       true

	       Current version: 0


Now observe the variable type of $rails_rake_task. It is changed to nil even though nothing in code is changed.


This simplified git project is used to demonstrate a similar issue observed in another rails application.

