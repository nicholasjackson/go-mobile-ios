require 'xcodebuilder'
require 'net/ftp'
require 'plist'
require 'keychain_osx'
require 'securerandom'

BUILD_DIR = Dir.pwd + "/build/Release-iphoneos"
PROVISONING_PROFILE = Dir.pwd + "/certs/Transform_Enterprise.mobileprovision"

namespace :framework do
	task :get do
		sh "go get github.com/nicholasjackson/go-mobile-framework"
	end

	task :build do
		sh "#{ENV['GOPATH']}/bin/gomobile bind -target=ios -o ./xcode/frameworks/mobilesdk.framework github.com/nicholasjackson/go-mobile-framework"
	end
end

namespace :app do
	builder = XcodeBuilder::XcodeBuilder.new do |config|
		config.app_name = "Transform"
		config.target = "TransformiPad"
		config.sdk = "iphoneos"
		config.project_file_path = "./xcode/TransformiPad.xcodeproj"
	end

	task :build do
	#	OSX::ProvisioningProfile.new(PROVISONING_PROFILE).install

	#	OSX::Keychain.temp do |keychain|
  	#	keychain.import('./certs/Transform.p12','ilikecake')
			builder.build
	#	end
	end

	task :createipa => ['build'] do
		OSX::ProvisioningProfile.new(PROVISONING_PROFILE).install

		OSX::Keychain.temp do |keychain|
			sh "xcrun -sdk iphoneos PackageApplication -v  #{BUILD_DIR}/Transform.app -o  #{BUILD_DIR}/Transform.ipa"
		end
	end
end
