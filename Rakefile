source_files = Rake::FileList.new("**/*.adoc") do |fl|
  fl.exclude("~*")
  fl.exclude(%r{\Aasciidoctor-fopub/})
  fl.exclude(%r{\Avendor/})
end

task default: :pdf

task pdf: source_files.ext('.pdf')
task html: source_files.ext('.html')

rule '.pdf' => '.adoc' do |t|
  dirname = File.dirname t.source
  basename = File.basename t.source, '.adoc'
  xml_file = "#{dirname}/#{basename}.xml"
  unless File.directory? "./asciidoctor-fopub"
	  sh "git clone https://github.com/asciidoctor/asciidoctor-fopub"
  end
	sh "bundle exec asciidoctor -b docbook -d book #{t.source} && ./asciidoctor-fopub/fopub #{xml_file} && open #{t.name}"
end

rule '.html' => '.adoc' do |t|
	sh "bundle exec asciidoctor --backend html5 -o #{t.name} #{t.source}"
end

task :watch do
  sh "bundle exec filewatcher '*.adoc' 'bundle exec rake'"
end
