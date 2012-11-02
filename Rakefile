task default: [:make_all]

debug = false

root = Dir.pwd

# the 'src' folder
src = ENV["src"] || "src"
src_dir = File.join(root, src)

# the 'bin' folder
bin = ENV["bin"] || "bin"
bin_dir = File.join(root, bin)

# where the server and webapp sources are
server_src = File.join(src_dir, "server")
webapp_src = File.join(src_dir, "webapp")

# where to put the compiled files of both the server and web app
server_bin = bin_dir
webapp_bin = File.join(bin_dir, "public/js/app")

if debug
  puts "root: #{root}"
  puts "src_dir: #{src_dir}"
  puts "bin_dir: #{bin_dir}"
  puts "server_src: #{server_src}"
  puts "webapp_src: #{webapp_src}"
  puts "server_bin: #{server_bin}"
  puts "webapp_bin: #{webapp_bin}"
end

# make a single file, the directory where to put it afterwards
def make(file_name, dir_out="bin")
  Dir.mkdir(dir_out) unless File.directory?(dir_out)
  res = `lispy #{file_name}.ls #{File.join(dir_out, file_name)}.js`
  puts res unless res.empty?
end

# take all the file names and make them into a directory
def make_all(files, dir_out="bin")
  files.each do |file|
    make(File.basename(file, ".ls"), dir_out)
  end
end

task :make_server do
  Dir.chdir(server_src) do |path|
    puts "making server in #{path}"
    make_all(Dir.glob("*.ls"), server_bin)
  end
end

task :make_webapp do
  Dir.chdir(webapp_src) do |path|
    puts "making webapp in #{path}"
    make_all(Dir.glob("*.ls"), webapp_bin)
  end
end

task :make_all => [:make_server, :make_webapp] do
  Dir.chdir(src_dir) do |path|
    puts "in #{path}"
    make_all(Dir.glob("*.ls"), bin_dir)
  end
end

task :clean do
  remove_us = [File.join(server_bin, "server.js")]
  remove_us = remove_us.concat Dir.glob("#{webapp_bin}/*.js")

  remove_us.each do |file|
    sh "rm #{file}" if File.exists? file
  end
end

task test: [:make_all] do
  sh "node #{File.join(server_bin, "server")}.js"
end
