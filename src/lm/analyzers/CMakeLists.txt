project(meta-lm-analyzers)

add_library(meta-lm-analyzers diff_analyzer.cpp)
target_link_libraries(meta-lm-analyzers meta-analyzers meta-language-model)
