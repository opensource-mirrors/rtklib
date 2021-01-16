# build with cmake as a subdirectory static library
option(RTKLIB_TRACE "if enable rtklib trace" OFF)

set(RTKLIB_DEFINES "-DENAGLO -DENAGAL -DENAQZS -DENACMP -DENAIRN -DNFREQ=5 -DNEXOBS=3 -DSVR_REUSEADDR ")
if(WIN32)
    set(RTKLIB_DEFINES "${RTKLIB_DEFINES} -DWIN32 -D_RTLDLL")
    set(LIBS "winmm.lib")
else()
    set(LIBS "rt")
endif(WIN32)

add_definitions(${RTKLIB_DEFINES})
set(RTKLIB_DEFINES ${RTKLIB_DEFINES} PARENT_SCOPE)

if (RTKLIB_TRACE)
    add_definitions("-DTRACE")
endif()

add_definitions("-Wall -ansi -pedantic -Wno-unused-but-set-variable -Wno-format-overflow -Wno-unused-variable")

include_directories(.)
aux_source_directory(. RTKLIB_SRC)
aux_source_directory(rcv RTKLIB_RCV_SRC)

add_library(rtklib ${RTKLIB_SRC} ${RTKLIB_RCV_SRC})
# if link with some libraries
target_link_libraries(rtklib
    pthread
    ${LIBS}
    m
)

