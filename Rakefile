Rake.application.options.trace_rules = true
require "rake/clean"

SOURCE_FILES = Rake::FileList.new("**/*.md", "**/*.markdown") do |fl|
  fl.exclude("~*")
  fl.exclude("README.md")
  fl.exclude(/^scratch\//)
  fl.exclude do |f|
    `git ls-files #{f}`.empty?
  end
end

def source_for_html(html_file)
  SOURCE_FILES.detect { |f|
    f.ext('') == html_file.pathmap("%{^outputs/,project/}X")
  }
end

task :default => :html
task :html => SOURCE_FILES.pathmap("%{^project/,outputs/}X.html")

# directoryの作成
directory "outputs"

rule ".html" => [->(f) { source_for_html(f) }, "outputs"] do |t|
  mkdir_p t.name.pathmap("%d")
  sh "pandoc -o #{t.name} #{t.source}"
end

CLEAN.include(SOURCE_FILES.pathmap("%{^project,outputs}X.html"))
CLOBBER.include("outputs")
