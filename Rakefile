require "rake"
require 'fileutils'
require 'merb-core/tasks/merb_rake_helper'

gems = %w[merb_activerecord merb_helpers merb_sequel merb_param_protection merb_test_unit merb_stories merb_screw_unit]
orm_gems = %w[merb_activerecord merb_sequel]

desc "Install it all"
task :install => "install:gems"

namespace :install do
  desc "Install the merb-plugins sub-gems"
  task :gems do
    gems.each do |dir|
      Dir.chdir(dir){ sh "rake install" }
    end
  end

  desc "Install the ORM merb-plugins sub-gems"
  task :orm do
    orm_gems.each do |dir|
       Dir.chdir(dir){ sh "rake install" }
    end
  end
end

desc "Bundle up all the merb-plugins gems"
task :bundle do
  mkdir_p "bundle"
  gems.each do |gem|
    File.open("#{gem}/Rakefile") do |rakefile|
      rakefile.read.detect {|l| l =~ /^GEM_VERSION\s*=\s*"(.*)"$/ }
      Dir.chdir(gem){ sh "rake package" }
      sh %{cp #{gem}/pkg/#{gem}-#{$1}.gem bundle/}
    end
  end
end

namespace :release do
  desc "Publish Merb More gem to RubyForge, one by one."
  task :gems => [ :bundle ] do
    gems.each do |gem|
      Dir.chdir(gem){ sh "rake release" }
    end
  end
end
