cmake_minimum_required(VERSION 3.0)
project(opcua-server)

set(UA_NAMESPACE_ZERO FULL CACHE STRING "" FORCE)
set(open62541_TOOLS_DIR "${CMAKE_SOURCE_DIR}/open62541/tools")
set(open62541_NODESET_DIR "${CMAKE_SOURCE_DIR}/open62541/deps/ua-nodeset")
add_subdirectory(open62541)

ua_generate_nodeset_and_datatypes(
        NAME "generated"
        FILE_CSV "${CMAKE_SOURCE_DIR}/../target/models/rootModel.Model.csv"
        FILE_BSD "${CMAKE_SOURCE_DIR}/../target/models/OUT/rootModel.Types.bsd"
        NAMESPACE_IDX 2
        FILE_NS "${CMAKE_SOURCE_DIR}/../target/models/OUT/rootModel.NodeSet2.xml"
        OUTPUT_DIR "${CMAKE_BINARY_DIR}/generated_nodes"
        INTERNAL
)

include_directories(${CMAKE_BINARY_DIR}/generated_nodes)

add_executable(server
        src/server.c
        ${UA_NODESET_ANIMAL_SOURCES}
        ${UA_TYPES_ANIMAL_SOURCES})
target_link_libraries(server open62541::open62541)

add_dependencies(server open62541-generator-ns-generated)