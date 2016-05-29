LIBS = ["tabulate"]
LIB_FILES = FileList[LIBS.map{|n| "src/#{n}/*.go"}]

TEST_PACKAGES = FileList[LIBS.map{|n| "src/#{n}/**/*_test.go"}].map { |e|
	name = e.pathmap("%n")
	name[0,name.length - 5]
}

TOOL_FILES = FileList["src/tools/*.go"]

#CODE_FILES = FileList["src/*"]
#	.map{ |e| e.pathmap("%n") } \
#	.reject{|e| LIBS.include?(e)} \
#	.reject{|e| e == "play"}
#
#my_packages = (LIBS.map{|n| "src/#{n}"} + SOLUTIONS)

DEPS = []

def go(args)
	ENV['GOPATH'] = Dir.pwd
	sh "go #{args}"
end

desc "Build all the executables"
task :build => :deps

desc "Run all the executables"
task :run

desc "Start doc server on port 8080"
task :doc do
	ENV['GOPATH'] = Dir.pwd
	sh "godoc -http=:8080"
end

task :default => :build

task :test do
	TEST_PACKAGES.each do |n|
		go "test #{n}"
	end
end

task :test_v do
	TEST_PACKAGES.each do |n|
		go "test -v #{n}"
	end
end

desc "Try code in play.go"
task :play do
	go "run src/play.go"
end

desc "Clean up any built binaries"
task :clean

TOOL_FILES.each do |n|
    exec = n.pathmap("%n")
    file exec => n do
        go "build #{n}"
	end
    file exec => LIB_FILES

	task :build => exec
	task :clean do
		rm n
	end
end

desc "Install any third-party dependencies"
task :deps do
	DEPS.each do |n|
		go("get #{n}")
	end
end
