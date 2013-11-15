require 'rspec/core/rake_task'
require 'rubygems/package_task'

Dir[File.expand_path("../*gemspec", __FILE__)].reverse.each do |gemspec_path|
  gemspec = eval(IO.read(gemspec_path))
  Gem::PackageTask.new(gemspec).define
end

require 'ffi_yajl/version'

desc "Run all specs in spec directory"
RSpec::Core::RakeTask.new(:spec) do |t|
  t.pattern = FileList['spec/**/*_spec.rb']
end

desc "Build it and ship it"
task :ship => [:clean, :gem] do
  sh("git tag #{Mixlib::ShellOut::VERSION}")
  sh("git push --tags")
  Dir[File.expand_path("../pkg/*.gem", __FILE__)].reverse.each do |built_gem|
    sh("gem push #{built_gem}")
  end
end

desc "install the gem locally"
task :install => [:package] do
  sh %{gem install pkg/#{gem_spec.name}-#{gem_spec.version}}
end

desc "remove build files"
task :clean do
  sh %Q{ rm -f pkg/*.gem }
end

task :default => :spec
