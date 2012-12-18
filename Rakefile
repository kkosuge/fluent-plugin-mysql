#!/usr/bin/env rake
require "bundler/gem_tasks"

module Bundler
  class GemHelper
    def build_gem
      file_name = nil
      sh("/usr/lib64/fluent/ruby/bin/gem build -V '#{spec_path}'") { |out, code|
        file_name = File.basename(built_gem_path)
        FileUtils.mkdir_p(File.join(base, 'pkg'))
        FileUtils.mv(built_gem_path, 'pkg')
        Bundler.ui.confirm "#{name} #{version} built to pkg/#{file_name}."
      }
      File.join(base, 'pkg', file_name)
    end

    def install_gem
      built_gem_path = build_gem
      out, _ = sh_with_code("/usr/lib64/fluent/ruby/bin/gem install '#{built_gem_path}' --local")
      raise "Couldn't install gem, run `/usr/lib64/fluent/ruby/bin/gem install #{built_gem_path}' for more detailed output" unless out[/Successfully installed/]
      Bundler.ui.confirm "#{name} (#{version}) installed."
    end
  end
end

require 'rake/testtask'
Rake::TestTask.new(:test) do |test|
  test.libs << 'lib' << 'test'
  test.pattern = 'test/**/test_*.rb'
  test.verbose = true
end

task :default => :test
