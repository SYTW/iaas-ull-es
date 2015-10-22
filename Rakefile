desc "Generar HTML (Instale pandoc)"
task :default do
  sh "pandoc -s README.md -o index.html"
end

desc "Generar LaTeX (Instale pandoc)"
task :latex do
  sh "pandoc -s README.md -o README.tex"
end

desc "Generar pdf (Instale LaTeX)"
task :pdf => [:latex] do
  sh "pdflatex README.tex"
end

task :clean do
  sh "rm -f *.log *.aux *.out README.html README.tex README.pdf"
end
