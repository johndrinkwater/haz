require "json"

def get_file_as_string(filename)
  data = ''
  f = File.open(filename, "r") 
  f.each_line do |line|
    data += line
  end
  return data
end

desc "Build it"
task :build do
  puts "Building …"
  json = get_file_as_string("capabilities.json")
  capabilities = JSON.parse(json)
  all_features = []
  capabilities["sections"].each do |section|
    section["features"].each do |feature|
      all_features.push(feature["name"])
    end
  end
  
  scss_path = "sass/screen.scss"
  scss = get_file_as_string(scss_path)
  
  output = File.open(scss_path, "w")
  output.write(scss.gsub /(\/\* FEATURES START \*\/).*(\/\* FEATURES END \*\/)/, "\\1%s\\2" % all_features.join(", "))
  output.close()
  sh "compass compile"
  puts "Built!"
end

desc "Deploy to server"
task :deploy do
  sh "compass compile && rsync -rtz --exclude .git --exclude .sass-cache --delete . pb@bc.pb.io:projects/haz/"
end