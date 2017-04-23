Rake.application.options.trace_rules = true

SOURCE_FILES = Rake::FileList.new("**/*.md", "**/*.markdown") do |fl|
  fl.exclude("~*")
  fl.exclude("README.md")
  fl.exclude(/^scratch\//)
  fl.exclude do |f|
    `git ls-files #{f}`.empty?
  end
end

def source_for_html(html_file)
  SOURCE_FILES.detect { |f| f.ext('') == html_file.ext('') }
end

task :default => :html
task :html => SOURCE_FILES.ext(".html")

rule ".html" => ->(f) { source_for_html(f) } do |t|
  sh "pandoc -o #{t.name} #{t.source}"
end
