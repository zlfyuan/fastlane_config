# This file contains the fastlane.tools configuration
# You can find the documentation at https://docs.fastlane.tools
#
# For a list of all available actions, check out
#
#     https://docs.fastlane.tools/actions
#
# For a list of all available plugins, check out
#
#     https://docs.fastlane.tools/plugins/available-plugins
#

# Uncomment the line if you want fastlane to automatically update itself
# update_fastlane

default_platform(:ios)


platform :ios do

  pgy_api_key = ENV['PGY_API_KEY']
  pgy_user_key = ENV['PGY_USER_KEY']
  pgy_app_key = ENV['PGY_APP_KEY']
  dingding_webhook = ENV['DINGDING_WEBHOOK']

  xcodeproj = ENV['XCODEPROJ']
  workspace = ENV['WORKSPACE']
  scheme_name = ENV['SCHEME']

  output_ipa_name = "#{scheme_name}.ipa"



  desc "发布release到AppStore"
  lane :release do |options|
    # 版本号设置
    # increment_build_number(
    #   build_number: options[:buildnumber]
    # )
    #  打包ipa文件
     buildapp(
      workspace: workspace,
      configuration: "Release",
      scheme: scheme_name,
      export_method: "app-store",
      output_name: output_ipa_name
    )
    upload_to_app_store
    notification(subtitle: "上传完成", message: "最新构建版本已经上传至AppStore connect")
    version = get_version_number(xcodeproj: xcodeproj)
    sh "sh /Users/zhongxiaoxi/sendUpdateNotifiy.sh #{scheme_name} #{version}"
  end


  desc "发布development到蒲公英"
  lane :development do |options|
    update_description = options[:message]

    UI.message(update_description)
    # 版本号设置
    # increment_build_number(
    #   build_number: options[:buildnumber]
    # )
    # 打包ipa文件
    buildapp(
      workspace: workspace,
      configuration: "Debug",
      scheme: scheme_name,
      export_method: "development",
      output_name: output_ipa_name
    )
    
    pgyer(
        api_key: pgy_api_key, 
        user_key: pgy_user_key,
        update_description: update_description
    )
  
    notifyworker(
        api_key: pgy_api_key, 
        app_key: pgy_app_key,
        updateDes: update_description,
        webhook: dingding_webhook
    )
  
    notification(subtitle: "上传完成", message: "最新测试包已经上传至蒲公英平台")
  end

  desc "发布ad-hoc到蒲公英"
  lane :beta do |options|
    update_description = options[:message]
    # 版本号设置
    # increment_build_number(
    #   build_number: options[:buildnumber]
    # )
    # 打包ipa文件
    buildapp(
      workspace: xcworkspace,
      configuration: "Release",
      scheme: scheme_name,
      export_method: "ad-hoc",
      output_name: output_ipa_name
    )
    
    pgyer(
        api_key: pgy_api_key, 
        user_key: pgy_user_key,
        update_description: update_description
    )
  
    notifyworker(
        api_key: pgy_api_key, 
        app_key: pgy_app_key,
        updateDes: update_description,
        webhook: "https://oapi.dingtalk.com/robot/send?access_token=b0d71fd971f6399c3959fb12fe38a185c295d18eeb90b4c990bdf75c5af2124d"
    )
  
    notification(subtitle: "上传完成", message: "最新测试包已经上传至蒲公英平台")
  end

  desc "build app"
  private_lane :buildapp do |options|
    gym(
      workspace: options[:workspace],
      configuration: options[:configuration],
      scheme: options[:scheme],
      clean: true,
      export_method: options[:export_method],
      output_directory: "./fastlane/build/",
      output_name: options[:output_name],
      sdk: "iphoneos"
      )
  end 
end