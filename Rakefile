require 'rake/testtask'
Rake::TestTask.new do |t|
  t.libs << "test"
  t.test_files = FileList['test/*_test.rb']
  t.verbose = true
end

desc 'Dumps output to a CSS file for testing'
task :debug do
  require 'sass'
  require './lib/bootstrap-sass/compass_functions'
  require './lib/bootstrap-sass/rails_functions'
  path = './vendor/assets/stylesheets'
  %w(bootstrap bootstrap-responsive).each do |file|
    engine = Sass::Engine.for_file("#{path}/#{file}.scss", syntax: :scss, load_paths: [path])
    File.open("./#{file}.css", 'w') { |f| f.write(engine.render) }
  end
end

desc 'Create full HTML example page for testing'
task :example do
  require 'sass'
  require './lib/bootstrap-sass/compass_functions'
  require './lib/bootstrap-sass/rails_functions'
  
  abort "abort: use template=<name> option" if ENV['template'].nil?
  abort "abort: empty template " if ENV['template'].empty?

  src_path = "./vendor/#{ENV['template']}/assets"
  dst_path = './example'
  abort "abort: template directory not found: #{src_path}" unless File.directory?(src_path)

  path = "#{src_path}/stylesheets"
  %w(bootstrap bootstrap-responsive).each do |file|
    engine = Sass::Engine.for_file("#{path}/#{file}.scss", syntax: :scss, load_paths: [path], debug_info: true)
    File.open("#{dst_path}/css/#{file}.css", 'w') { |f| f.write(engine.render) }
  end

  %w(images javascripts).each do |folder|
    FileUtils.cp_r "#{src_path}/#{folder}/.", "#{dst_path}/#{folder}"
  end
  puts "Ok, to view the example run: ruby example/app.rb and navigate to http://localhost:4567"
end 

task default: :test