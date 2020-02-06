                                
set(SOURCE_FILES
		tst_dwt.cpp

	)





add_executable(wavelibLibTests ${SOURCE_FILES} )

add_test(NAME wavelibLibTests WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/test  COMMAND wavelibLibTests)

add_dependencies(wavelibLibTests libwavelib)
target_link_libraries(wavelibLibTests libwavelib)

target_include_directories(wavelibLibTests PUBLIC
							${CMAKE_CURRENT_SOURCE_DIR}/../../header
                            )
                            

install(TARGETS wavelibLibTests
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION tests
    ARCHIVE DESTINATION tests
)
