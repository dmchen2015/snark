SET( PROJECT "imaging" )
SET( TARGET_NAME snark_${PROJECT} )
SET( DIR ${SOURCE_CODE_BASE_DIR}/${PROJECT} )

FILE( GLOB source ${SOURCE_CODE_BASE_DIR}/${PROJECT}/*.cpp )
FILE( GLOB includes ${SOURCE_CODE_BASE_DIR}/${PROJECT}/*.h )

FILE( GLOB camera_source ${SOURCE_CODE_BASE_DIR}/${PROJECT}/camera/*.cpp )
FILE( GLOB camera_includes ${SOURCE_CODE_BASE_DIR}/${PROJECT}/camera/*.h )

FILE( GLOB color_source ${SOURCE_CODE_BASE_DIR}/${PROJECT}/color/*.cpp )
FILE( GLOB color_includes ${SOURCE_CODE_BASE_DIR}/${PROJECT}/color/*.h )

FILE( GLOB cvmat_source ${SOURCE_CODE_BASE_DIR}/${PROJECT}/cv_mat/*.cpp )
FILE( GLOB cvmat_includes ${SOURCE_CODE_BASE_DIR}/${PROJECT}/cv_mat/*.h )

FILE( GLOB cvmat_detail_includes ${SOURCE_CODE_BASE_DIR}/${PROJECT}/cv_mat/detail/*.h )
FILE( GLOB cvmat_detail_source ${SOURCE_CODE_BASE_DIR}/${PROJECT}/cv_mat/detail/*.cpp )

if( snark_build_imaging_stereo ) # todo: add proper cmake to stereo
    file( GLOB stereo_source ${SOURCE_CODE_BASE_DIR}/${PROJECT}/stereo/*.cpp )
    file( GLOB stereo_includes ${SOURCE_CODE_BASE_DIR}/${PROJECT}/stereo/*.h )
endif( snark_build_imaging_stereo )
    
FILE( GLOB vegetation_source ${SOURCE_CODE_BASE_DIR}/${PROJECT}/vegetation/*.cpp )
FILE( GLOB vegetation_includes ${SOURCE_CODE_BASE_DIR}/${PROJECT}/vegetation/*.h )

ADD_LIBRARY( ${TARGET_NAME} ${source} ${includes}
                            ${camera_source} ${camera_includes}
                            ${color_source} ${color_includes}
                            ${cvmat_source} ${cvmat_includes}
                            ${cvmat_detail_source} ${cvmat_detail_includes}
                            ${stereo_source} ${stereo_includes}
                            ${vegetation_source} ${vegetation_includes} )
SET_TARGET_PROPERTIES( ${TARGET_NAME} PROPERTIES ${snark_LIBRARY_PROPERTIES} )
TARGET_LINK_LIBRARIES( ${TARGET_NAME} ${comma_ALL_LIBRARIES} ${comma_ALL_EXTERNAL_LIBRARIES} snark_timing snark_math ${OpenCV_LIBS} tbb ${pgrey_libs} )
#TARGET_LINK_LIBRARIES( ${TARGET_NAME} ${comma_ALL_LIBRARIES} ${comma_ALL_EXTERNAL_LIBRARIES} ${OpenCV_LIBS} tbb fftw3 ${pgrey_libs} )

INSTALL( FILES ${includes} DESTINATION ${snark_INSTALL_INCLUDE_DIR}/${PROJECT} )
INSTALL( FILES ${camera_includes} DESTINATION ${snark_INSTALL_INCLUDE_DIR}/${PROJECT}/camera )
INSTALL( FILES ${color_includes} DESTINATION ${snark_INSTALL_INCLUDE_DIR}/${PROJECT}/color )
INSTALL( FILES ${cvmat_includes} DESTINATION ${snark_INSTALL_INCLUDE_DIR}/${PROJECT}/cv_mat )

if( snark_build_imaging_stereo ) # todo: add proper cmake to stereo
    install( FILES ${stereo_includes} DESTINATION ${snark_INSTALL_INCLUDE_DIR}/${PROJECT}/stereo )
endif( snark_build_imaging_stereo )

INSTALL( FILES ${vegetation_includes} DESTINATION ${snark_INSTALL_INCLUDE_DIR}/${PROJECT}/vegetation )

INSTALL(
    TARGETS ${TARGET_NAME}
    RUNTIME DESTINATION ${snark_INSTALL_BIN_DIR} COMPONENT Runtime     # .exe, .dll
    LIBRARY DESTINATION ${snark_INSTALL_LIB_DIR} COMPONENT Runtime     # .so, mod.dll
    ARCHIVE DESTINATION ${snark_INSTALL_LIB_DIR} COMPONENT Development # .a, .lib
)

IF( snark_BUILD_APPLICATIONS )
    ADD_SUBDIRECTORY( applications )
    ADD_SUBDIRECTORY( examples )
ENDIF( snark_BUILD_APPLICATIONS )

IF( snark_BUILD_TESTS )
    ADD_SUBDIRECTORY( test )
ENDIF( snark_BUILD_TESTS )
