CONFIGURE_FILE(
    ${CMAKE_CURRENT_SOURCE_DIR}/CLandmarkConfig.h.in
    ${CMAKE_CURRENT_BINARY_DIR}/CLandmarkConfig.h
)

INCLUDE_DIRECTORIES(
    ${CMAKE_SOURCE_DIR}/3rd_party/CImg-1.5.6
    ${CMAKE_SOURCE_DIR}/3rd_party/rapidxml-1.13
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
)

SET(clandmark_libs
    flandmark
    ${OpenCV_LIBRARIES}
)

FIND_PACKAGE(OpenMP)
IF(OPENMP_FOUND)
    SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    LIST(APPEND clandmark_libs gomp)
ELSE(OPENMP_FOUND)
    SET(USE_OPENMP OFF)
ENDIF(OPENMP_FOUND)

ADD_DEFINITIONS(-fPIC)

SET(flandmark_srcs
    Flandmark.cpp
    CSparseLBPFeatures.cpp
    CSparseLBPAppearanceModel.cpp
    CDisplacementDeformationCost.cpp
    CNormalizedEuclideanLoss.cpp
    CTableLoss.cpp
    CZeroLoss.cpp
    CTreeMaxSumSolver.cpp
)

ADD_LIBRARY(flandmark STATIC ${flandmark_srcs})

SET(clandmark_srcs
    CLandmark.cpp
    CFeaturePool.cpp
    CFeatures.cpp
    CAppearanceModel.cpp
    CDeformationCost.cpp
    CLoss.cpp
    CMaxSumSolver.cpp
    CXMLInOut.cpp
)

ADD_LIBRARY(clandmark SHARED ${clandmark_srcs})
TARGET_LINK_LIBRARIES(clandmark ${clandmark_libs})

SET(clandmark_headers
    base64.h
    CAppearanceModel.h
    CDAGMaxSumSolver.h
    CDeformationCost.h
    CDisplacementDeformationCost.h
    CFeaturePool.h
    CFeatures.h
    CLandmark.h
    CLoss.h
    CMaxSumSolver.h
    CNormalizedEuclideanLoss.h
    CSparseLBPAppearanceModel.h
    CSparseLBPFeatures.h
    CTableLoss.h
    CTimer.h
    CTreeMaxSumSolver.h
    CTypes.h
    CXMLInOut.h
    CZeroLoss.h
    Flandmark.h
    helpers.h
    msvc-compat.h
    ${CMAKE_SOURCE_DIR}/3rd_party/CImg-1.5.6/CImg.h
    ${CMAKE_SOURCE_DIR}/3rd_party/rapidxml-1.13/rapidxml.hpp
    ${CMAKE_SOURCE_DIR}/3rd_party/rapidxml-1.13/rapidxml_iterators.hpp
    ${CMAKE_SOURCE_DIR}/3rd_party/rapidxml-1.13/rapidxml_print.hpp
    ${CMAKE_SOURCE_DIR}/3rd_party/rapidxml-1.13/rapidxml_utils.hpp
)

INSTALL(
    FILES ${clandmark_headers}
    DESTINATION ${INCLUDE_DIR} COMPONENT Headers
)
INSTALL(
    TARGETS clandmark
    LIBRARY DESTINATION ${LIBRARY_DIR} COMPONENT Library
)
