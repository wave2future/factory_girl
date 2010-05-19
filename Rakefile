require 'rubygems'
require 'rake'
require 'rake/rdoctask'
require 'rcov/rcovtask'
require 'date'

require 'spec/rake/spectask'
require 'cucumber/rake/task'

desc 'Default: run the specs and features.'
task :default do
  %w(2.1 2.3 3.0).each do |version|
    system("RAILS_VERSION=#{version} rake -s spec features;")
  end
end

Spec::Rake::SpecTask.new do |t|
  t.spec_opts = ['--options', "spec/spec.opts"]
end

desc 'Performs code coverage on the factory_girl plugin.'
Rcov::RcovTask.new do |t|
  t.test_files = FileList['spec/*_spec.rb']
  t.verbose = true
end

desc 'Generate documentation for the factory_girl plugin.'
Rake::RDocTask.new(:rdoc) do |rdoc|
  rdoc.rdoc_dir = 'rdoc'
  rdoc.title    = 'Factory Girl'
  rdoc.options << '--line-numbers' << "--main" << "README.rdoc"
  rdoc.rdoc_files.include('README.rdoc')
  rdoc.rdoc_files.include('CONTRIBUTION_GUIDELINES.rdoc')
  rdoc.rdoc_files.include('lib/**/*.rb')
end

desc 'Update documentation on website'
task :sync_docs => 'rdoc' do
  `rsync -ave ssh rdoc/ dev@dev.thoughtbot.com:/home/dev/www/dev.thoughtbot.com/factory_girl`
end

desc "Clean files generated by rake tasks"
task :clobber => [:clobber_rdoc, :clobber_rcov]

Cucumber::Rake::Task.new(:features) do |t|
  t.fork = true
  t.cucumber_opts = ['--format', (ENV['CUCUMBER_FORMAT'] || 'progress')]
end
