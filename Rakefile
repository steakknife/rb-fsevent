# -*- encoding: utf-8 -*-
require 'bundler'
Bundler::GemHelper.install_tasks

require 'rspec/core/rake_task'
RSpec::Core::RakeTask.new(:spec)
task :default => ['bin/fsevent_watch', :spec]

file 'bin/fsevent_watch' => [:bin, 'ext/build/fsevent_watch'] do
  cp 'ext/build/fsevent_watch', 'bin/fsevent_watch'
end

file 'ext/build/fsevent_watch' do
  cd 'ext' do
    sh 'rake build'
  end
end

file :bin do
  mkdir_p 'bin'
end

namespace(:spec) do
  desc "Run all specs on multiple ruby versions"
  task(:portability => 'bin/fsevent_watch') do
    versions = %w[2.2.2 2.3.0-dev rbx-2.5.5 jruby-1.7.9]
    versions.each do |version|
      # system <<-BASH
      #   bash -c 'source ~/.rvm/scripts/rvm;
      #            rvm #{version};
      #            echo "--------- version #{version} ----------\n";
      #            bundle install;
      #            rake spec'
      # BASH
      system <<-BASH
        bash -c 'export PATH="$HOME/.rbenv/bin:$PATH";
                 [[ `which rbenv` ]] && eval "$(rbenv init -)";
                 [[ ! -a $HOME/.rbenv/versions/#{version} ]] && rbenv install #{version};
                 rbenv shell #{version};
                 rbenv which bundle 2> /dev/null || gem install bundler;
                 bundle install;
                 rake spec;'
      BASH
    end
  end
end
