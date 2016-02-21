desc "Build ~/Postbird.nw and ~/Postbird.app"

task :build do
  exec "./build_files/build.sh"
end

desc "Build ~/Postbird.dmg"

task :build_dmg do
  file_name = "Postbird-0.4.3.dmg"
  File.delete("~/#{file_name}") if File.exist?("~/#{file_name}")

  exec %{
    ./build_files/yoursway-create-dmg/create-dmg \
      --volname "Postbird 0.4.3" \
      --volicon "./build_files/icon.icns" \
      --icon Postbird.app 50 50 \
      --app-drop-link 300 50 \
      ~/#{file_name} \
      ~/Postbird.app
  }
end

task :build_dev do
  source = ENV['PWD']
  target = "~/Postbird.app"

  # rm -f #{target}/Contents/Resources/icon.icns
  # ln -s #{source}/build_files/icon.icns     #{target}/Contents/Resources

  puts %x{
    rm -rf #{target}/Contents/Resources/app.nw
    rm -f #{target}/Contents/Resources/Credits.html
    rm -f #{target}/Contents/info.plist
    rm -f #{target}/Contents/Resources/icon.icns

    ln -s #{source}                           #{target}/Contents/Resources/app.nw
    ln -s #{source}/build_files/Credits.html  #{target}/Contents/Resources
    ln -s #{source}/build_files/info.plist    #{target}/Contents
    ln -s #{source}/build_files/icon.icns     #{target}/Contents/Resources
  }
  puts "Complete"
end

desc "Rebuild npm native extensions for electron"
task :rebuild_ext do
  if RUBY_PLATFORM =~ /darwin/
    system "PATH=$PATH:/Applications/Postgres.app/Contents/Versions/9.4/bin ./node_modules/.bin/electron-rebuild -n 47"
    system "mv node_modules/fibers/bin/darwin-x64-v8-4.6 node_modules/fibers/bin/darwin-x64-v8-4.7"
  else
    system "./node_modules/.bin/electron-rebuild -n 47"
    system "mv node_modules/fibers/bin/linux-x64-v8-4.6 node_modules/fibers/bin/linux-x64-v8-4.7"
  end
end