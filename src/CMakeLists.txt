set(SOURCE_FILES
  conv.c
  cwt.c
  cwtmath.c
  hsfft.c
  real.c
  wavefilt.c
  wavefunc.c
  wavelib.c
  wtmath.c
)

set(EXT_HEADER_FILES
  ../include/wavelib/wauxlib.h
  ../include/wavelib/wavelib.h
)

add_library(libwavelib STATIC ${SOURCE_FILES})

target_include_directories(libwavelib PUBLIC
  $<BUILD_INTERFACE:${PROJECT_WIDE_INCLUDE}>
  $<INSTALL_INTERFACE:include>
)

install(TARGETS libwavelib
  EXPORT wavelib
  LIBRARY DESTINATION lib
  INCLUDES DESTINATION include
)

install(FILES ${EXT_HEADER_FILES}
  DESTINATION include/wavelib
)
