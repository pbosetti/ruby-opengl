#-*-ruby-*-
#
# Copyright (C) 2006 John M. Gabriele <jmg3000@gmail.com>
#
# This program is distributed under the terms of the MIT license.
# See the included MIT-LICENSE file for the terms of this license.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS
# OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
# MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
# IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
# CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
# TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
# SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

require 'rake'
require 'rake/clean'
require 'rubygems/package_task'
require 'rake/testtask'
require 'mkrf/rakehelper'


CLEAN.include("ext/gl*/Rakefile", "ext/*/mkrf.log", "ext/*/*.so", 
              "ext/**/*.bundle", "lib/*.so", "lib/*.bundle", "ext/*/*.o{,bj}", 
              "ext/*/*.lib", "ext/*/*.exp", "ext/*/*.pdb",
              "pkg")

setup_extension('gl', 'gl')
setup_extension('glu', 'glu')
setup_extension('glut', 'glut')

desc 'Does a full compile'
task :default => [:gl, :glu, :glut, :fixpermissions]

task :fixpermissions do
  # fix wrong lib permissions (mkrf bug ?)
  Dir["lib/*.so","lib/*.bundle"].each do |fname|
    File.chmod(0755,fname)
  end
end

task :extension => :default


desc 'Runs unit tests.'
Rake::TestTask.new do |t|
    t.libs << "test"
    t.test_files = FileList['test/tc_*.rb']
    t.verbose = true
end

desc 'Runs unit tests.'
task :test_all => [:test]




############# gems #############

# common specification for source and binary gems
spec = Gem::Specification.new do |s|
    s.name              = "ruby-opengl2"
    s.version           = "0.60.6"
    s.authors           = [ "Alain Hoang", "Jan Dvorak", "Minh Thu Vo", "James Adam", "Paolo Bosetti" ]
    s.homepage          = "http://github.com/pbosetti/ruby-opengl"
    s.email             = "p4010@me.com"
    s.rubyforge_project = 'ruby-opengl2'
    s.platform          = Gem::Platform::RUBY
    s.summary           = "OpenGL Interface for Ruby"
    s.description       = "This is a modernization of the glorious but unmaintained ruby-opengl version, aimed at making it compatible with ruby 1.9.x series. At the moment, it successfully compiles on OS X Mountain Lion and Debian 6.0.3"
    s.require_path      = "lib"
    s.has_rdoc          = false
    s.files             = FileList.new("{lib,ext,examples,test}/**/*") do |fl|
                            fl.exclude(/Rakefile/)
                          end
    s.extensions        = ['Rakefile']
    
    if s.respond_to? :specification_version then
      current_version = Gem::Specification::CURRENT_SPECIFICATION_VERSION
      s.specification_version = 3

      if Gem::Version.new(Gem::RubyGemsVersion) >= Gem::Version.new('1.2.0') then
        s.add_runtime_dependency(%q<mkrf>, [">= 0.2.3"])
        s.add_runtime_dependency(%q<rake>, [">= 10.0"])
      else
        s.add_dependency(%q<mkrf>, [">= 0.2.3"])
        s.add_dependency(%q<rake>, [">= 10.0"])
      end
    else
      s.add_dependency(%q<mkrf>, [">= 0.2.3"])
      s.add_dependency(%q<rake>, [">= 10.0"])
    end
end

# Create a task for creating a ruby source gem
Gem::PackageTask.new(spec) do |pkg|
  pkg.need_tar = true
end
