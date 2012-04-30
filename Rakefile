desc 'list available rake tasks'
task :default do
	puts 'There is no default task for your own safety.' 
	puts 'Try one of these specific tasks:'
	sh 'rake --tasks --silent'
end

#generate and deploy tasks
desc 'build development site using Jekyll'
task :devgenerate do
	# builds the site using Jekyll
	# Jekyll will get use URLs passed to command line
	# Jekyll will take other configurations from _config.yml
	puts 'Generating the development site.'
	Rake::Task["assets"].invoke
	sh "jekyll --base-url http://lincolnmullen.com/dev/jsr/ --url http://lincolnmullen.com/dev/jsr --no-auto"
	Rake::Task["tidy"].invoke
	puts 'Successfully built site!'
end

desc 'deploy development site with rsync'
task :devdeploy do
	# uploads preview version via SSH and rsync
	# does NOT delete other files on the server
	puts 'Deploying dev preview to <lincolnmullen.com/dev/jsr/> with rsync.'
	sh 'rsync -avze ssh _site/ lam:/home/lincolnm/public_html/dev/jsr/'
	puts 'Successfully deployed site!'
end

desc 'generate and deploy the development site'
task :dev => [:devgenerate, :devdeploy] do
	puts 'Generated and deployed the development site in one step.'
end

desc 'generate staging site'
task :staginggenerate do
	# builds the site using Jekyll
	# Jekyll will get use URLs passed to command line
	# Jekyll will take other configurations from _config.yml
	puts 'Generating the staging site.'
	Rake::Task["assets"].invoke
	sh "jekyll --base-url http://staging.jsr.fsu.edu/ --url http://staging.jsr.fsu.edu --no-auto"
	Rake::Task["tidy"].invoke
	puts 'Successfully built staging site!'
end

desc 'generate production site'
task :production_generate do
	# builds the site using Jekyll
	# Jekyll will get use URLs passed to command line
	# Jekyll will take other configurations from _config.yml
	puts 'Generating the production site.'
	Rake::Task["assets"].invoke
	sh "jekyll --base-url http://jsr.fsu.edu/ --url http://jsr.fsu.edu --no-auto"
	Rake::Task["tidy"].invoke
	puts 'Successfully built production site!'
end

desc 'preview site locally'
task :preview do
	# Generates the site locally, launches a server, auto regenerates
	# Jekyll gets URLS from options passed to command line
	# Other options are taken from _config.yml
	Rake::Task["assets"].invoke
	puts 'Previewing site with a local server.'
	puts 'See the site at <http://localhost:4000/>.'
	puts 'Use CTRL+C to interrupt.'
	sh 'jekyll --auto --server --base-url / --url http://localhost:4000'
	# after the server is interrupted
	puts 'Finished previewing the site locally.'
end

#helper functions

desc 'assemble Bootstrap and other Javascripts'
task :js do
	# concatenate just the scripts that we need 
	sh 'cat ./_bootstrap/js/bootstrap-dropdown.js ./_bootstrap/js/bootstrap-collapse.js ./_bootstrap/js/bootstrap-tooltip.js ./_bootstrap/js/bootstrap-modal.js ./_bootstrap/js/bootstrap-transition.js ./assets/audio-player/audio-player.js ./assets/js/footnotify.js > ./_bootstrap/jsr.tmp.js'
	# compress the JavaScript and copy it to our js directory
	sh 'uglifyjs -nc ./_bootstrap/jsr.tmp.js > ./assets/js/jsr.min.js'
	# remove the temporary file
	sh 'rm ./_bootstrap/jsr.tmp.js'
end

desc 'copy images'
task :img do
	# copy the Bootstrap images to our image directory
	sh 'cp ./_bootstrap/img/* ./assets/img/'
end

desc 'compile CSS from LESS '
task :css do
	# compile, compress, and copy main Bootstrap CSS
	sh 'lessc --compress ./_bootstrap/less/bootstrap.less > ./assets/css/bootstrap.min.css'
	# compile, compress, and copy responsive layout CSS
	sh 'lessc --compress ./_bootstrap/less/responsive.less > ./assets/css/bootstrap-responsive.min.css'
end

desc 'update all assets'
task :assets => [:img, :js, :css] do
	puts 'Successfully updated all assets.'
end

desc 'tidy HTML in _site'
task :tidy do
	puts 'Tidying the HTML in _site/.'
	sh 'find ./_site/ -type f -name "*.html" -exec tidy -config _tidy.config -modify -i {} \;'
	puts 'Done tidying.'
end
