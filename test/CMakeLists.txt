find_package(GTest)

set(test_SOURCES
    run-all-tests.cpp
    test-helper.cpp
    test-TUNGraph.cpp test-TNGraph.cpp
    test-TNEGraph.cpp test-TNEANet.cpp
    test-TNodeNet.cpp test-TNodeEDatNet.cpp test-TNodeEdgeNet.cpp
    test-TTable.cpp
    test-TUndirNet.cpp test-TDirNet.cpp
    test-TMMNet.cpp
    test-TModeNet.cpp
    test-TCrossNet.cpp
    test-multimodal.cpp
    test-subgraph.cpp
    test-TStrPool.cpp
    test-TSysTm.cpp
    test-ggen.cpp
    test-gio.cpp
    test-alg.cpp
    test-triad.cpp
    test-THash.cpp
    test-THashSet.cpp
    test-TAttr.cpp
    test-flow.cpp
    test-randwalk.cpp
    test-priority-queue.cpp
    test-sim.cpp
)
if(CMAKE_HOST_WIN32)
else()
    set(test_SOURCES ${test_SOURCES} 
        test-bfsdfs.cpp
        test-cncom.cpp
        test-gviz.cpp
    )
endif(CMAKE_HOST_WIN32)


if(GTest_FOUND)
add_executable(run-all-tests
    ${test_SOURCES}
)
target_include_directories(run-all-tests SYSTEM
    PUBLIC ${SNAPCORE_INCLUDE_DIRS}
    PUBLIC ${GLIBCORE_INCLUDE_DIRS}
    PRIVATE ${GTEST_INCLUDE_DIRS}
)
target_link_libraries(run-all-tests
    snapcore
    ${GTEST_BOTH_LIBRARIES}
)
else()
message("Google Test is not found on your computer. Skip.")
endif(GTest_FOUND)