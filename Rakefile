require 'rake/clean'
require 'less'
require 'jsmin'

##### Bootstrap CSS

variables = File.expand_path('src/variables.less')

bs     = File.expand_path('public/css/bootstrap.css')
bsmin  = File.expand_path('public/css/bootstrap.min.css')
bsr    = File.expand_path('public/css/bootstrap-responsive.css')
bsrmin = File.expand_path('public/css/bootstrap-responsive.min.css')

CLEAN.include(bs, bsmin, bsr, bsrmin)

file 'public/css/bootstrap.css' => (FileList['submodules/bootstrap/less/*'] + [variables, __FILE__]) do
  Dir.chdir('submodules/bootstrap/less') do
    mv 'variables.less', 'variables.less.bak'
    cp variables, 'variables.less'

    parser = Less::Parser.new
    
    File.open(bs, 'wb')     {|out| out.write(parser.parse(IO.read('bootstrap.less')).to_css)}
    File.open(bsmin, 'wb')  {|out| out.write(parser.parse(IO.read('bootstrap.less')).to_css(:compress => true))}
    File.open(bsr, 'wb')    {|out| out.write(parser.parse(IO.read('responsive.less')).to_css)}
    File.open(bsrmin, 'wb') {|out| out.write(parser.parse(IO.read('responsive.less')).to_css(:compress => true))}

    mv 'variables.less.bak', 'variables.less'
  end
end

task :bootstrap => ['public/css/bootstrap.css']

##### Bootstrap JS

Dir['submodules/bootstrap/js/*.js'].each do |js|
  target = "public/js/#{File.basename(js)}"
  CLEAN.include(target)
  file target => js do
    cp js, target
  end
  task :bootstrap => target
end

##### SHJS

shjs = (
  Dir['submodules/gherkin-syntax-highlighters/shjs/shjs-0.6-src/sh_main.min.js'] +
  Dir['submodules/gherkin-syntax-highlighters/shjs/shjs-0.6-src/lang/sh_{cpp,csharp,java,javascript,properties,python,ruby,scala,sh,xml}.min.js'] +
  Dir['submodules/gherkin-syntax-highlighters/shjs/sh_{clojure,lua}.js'] +
  Dir['submodules/gherkin-syntax-highlighters/shjs/sh_gherkin_{en}.js']
)

CLEAN.include('public/js/shjs.js')
file 'public/js/shjs.js' => shjs do
  File.open('public/js/shjs.js', 'w') do |io|
    shjs.each do |js|
      io.write(';')
      io.write(JSMin.minify(IO.read(js)))
    end
  end
end
task :shjs_gherkin => 'public/js/shjs.js'

task :default => [:bootstrap, :shjs_gherkin]