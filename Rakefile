require 'bundler/setup'
require 'rake'
require 'rspec/core/rake_task'
require 'foodcritic'
require 'rubocop/rake_task'


task :default => [:test]

RSpec::Core::RakeTask.new(:spec) do |t|
  t.pattern = "./test/spec{,/*/**}/*_spec.rb"
  t.ruby_opts = "-I./test/spec"
end

FoodCritic::Rake::LintTask.new

Rubocop::RakeTask.new(:rubocop) do |task|
  task.patterns = ['{providers,resources,templates,test}/**/*.rb', '*.rb']
end

test_suite = [ :foodcritic, :spec, :rubocop ]

begin
  require 'kitchen/rake_tasks'
  Kitchen::RakeTasks.new
  test_suite << "kitchen:all"
rescue LoadError
  puts '>>>>> Kitchen gem not loaded, omitting tasks' unless ENV['CI']
end

desc "Run all tests"
task :test do
  test_suite.each do |task|
    Rake::Task[task].invoke
  end
end

desc "Runs knife cookbook test against all the cookbooks"
task :knife_test do
  sh "knife cookbook test #{File.basename(File.expand_path("..", __FILE__))} -o ../."
end
