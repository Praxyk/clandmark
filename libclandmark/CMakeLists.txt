INCLUDE_DIRECTORIES(
    ${CMAKE_SOURCE_DIR}/3rd_party/CImg-1.5.6
    ${CMAKE_SOURCE_DIR}/3rd_party/rapidxml-1.13
    ${CMAKE_CURRENT_SOURCE_DIR}/include
    ${CMAKE_CURRENT_BINARY_DIR}
)

FIND_PACKAGE(OpenMP)
IF(OPENMP_FOUND)
    SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
ELSE(OPENMP_FOUND)
    SET(USE_OPENMP OFF)
ENDIF(OPENMP_FOUND)

ADD_DEFINITIONS(-fPIC)

SET(clandmark_srcs
    Flandmark.cpp
    CSparseLBPFeatures.cpp
    CSparseLBPAppearanceModel.cpp
    CDisplacementDeformationCost.cpp
    CNormalizedEuclideanLoss.cpp
    CTableLoss.cpp
    CZeroLoss.cpp
    CTreeMaxSumSolver.cpp
    CLandmark.cpp
    CFeaturePool.cpp
    CFeatures.cpp
    CAppearanceModel.cpp
    CDeformationCost.cpp
    CLoss.cpp
    CMaxSumSolver.cpp
    CXMLInOut.cpp
)

ADD_LIBRARY(clandmark STATIC ${clandmark_srcs})
IF(OPENMP_FOUND)
    TARGET_LINK_LIBRARIES(clandmark gomp)
ENDIF(OPENMP_FOUND)

SET(clandmark_headers
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/base64.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/CAppearanceModel.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/CDAGMaxSumSolver.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/CDeformationCost.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/CDisplacementDeformationCost.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/CFeaturePool.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/CFeatures.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/CLandmark.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/CLoss.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/CMaxSumSolver.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/CNormalizedEuclideanLoss.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/CSparseLBPAppearanceModel.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/CSparseLBPFeatures.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/CTableLoss.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/CTimer.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/CTreeMaxSumSolver.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/CTypes.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/CXMLInOut.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/CZeroLoss.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/Flandmark.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/helpers.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/clandmark/msvc-compat.h
)

SET(third_party_headers
    ${CMAKE_SOURCE_DIR}/3rd_party/CImg-1.5.6/CImg.h
    ${CMAKE_SOURCE_DIR}/3rd_party/rapidxml-1.13/rapidxml.hpp
    ${CMAKE_SOURCE_DIR}/3rd_party/rapidxml-1.13/rapidxml_iterators.hpp
    ${CMAKE_SOURCE_DIR}/3rd_party/rapidxml-1.13/rapidxml_print.hpp
    ${CMAKE_SOURCE_DIR}/3rd_party/rapidxml-1.13/rapidxml_utils.hpp
)

INSTALL(
    FILES ${clandmark_headers}
    DESTINATION ${INCLUDE_DIR}/clandmark COMPONENT Headers
)
INSTALL(
    FILES ${third_party_headers}
    DESTINATION ${INCLUDE_DIR} COMPONENT Headers
)
INSTALL(
    TARGETS clandmark
    ARCHIVE DESTINATION ${LIBRARY_DIR} COMPONENT Library
)
