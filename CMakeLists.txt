cmake_minimum_required(VERSION 3.22)
project(Apprehension VERSION 1.3.0)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include(FetchContent)
FetchContent_Declare(JUCE
  GIT_REPOSITORY https://github.com/juce-framework/JUCE.git
  GIT_TAG 8.0.4
  GIT_SHALLOW TRUE)
FetchContent_MakeAvailable(JUCE)

juce_add_plugin(Apprehension
  COMPANY_NAME "Reyaan."
  PLUGIN_MANUFACTURER_CODE Reya
  PLUGIN_CODE Appn
  FORMATS VST3
  PRODUCT_NAME "Apprehension"
  BUNDLE_ID com.reyaan.apprehension
  IS_SYNTH FALSE
  NEEDS_MIDI_INPUT FALSE
  NEEDS_MIDI_OUTPUT FALSE
  VST3_CATEGORIES Fx Delay
  COPY_PLUGIN_AFTER_BUILD FALSE)

# Logo is always embedded. Drop a cursive font file named assets/font.ttf (any free font you have the licence for)
# next to it and it is embedded and used for all the script text automatically.
set(APPRENSION_ASSETS assets/logo.png assets/manual.html)
if(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/assets/font.ttf")
  list(APPEND APPRENSION_ASSETS assets/font.ttf)
  target_compile_definitions(Apprehension PUBLIC APPREHENSION_CUSTOM_FONT=1)
endif()
juce_add_binary_data(ApprehensionAssets SOURCES ${APPRENSION_ASSETS})
set_target_properties(ApprehensionAssets PROPERTIES POSITION_INDEPENDENT_CODE TRUE)

target_sources(Apprehension PRIVATE src/PluginProcessor.cpp src/PluginProcessor.h src/PluginEditor.h src/Theme.h src/dsp.h src/keydetect.h src/params.h src/presets.h src/HelpText.h)
target_include_directories(Apprehension PRIVATE src)
target_compile_definitions(Apprehension PUBLIC
  JUCE_WEB_BROWSER=0 JUCE_USE_CURL=0 JUCE_VST3_CAN_REPLACE_VST2=0)
target_link_libraries(Apprehension PRIVATE ApprehensionAssets juce::juce_audio_processors)
