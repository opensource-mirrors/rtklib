# build with cmake as a subdirectory static library

add_definitions("-Wall -ansi -pedantic -Wno-unused-but-set-variable")

add_definitions("-D_RTLDLL -DNO_STRICT -DENAGLO -DENAQZS -DENAGAL -DENACMP -DENAIRN -DNFREQ=3 -DNEXOBS=3 -DEXTLEX")
add_definitions("-DSVR_REUSEADDR")

if(WIN32)
    add_definitions("-DWIN32")
    set(LIBS "winmm.lib")
else()
    set(LIBS "rt")
endif(WIN32)


# if enable rtklib trace for debug
# add_definitions("-DTRACE")

include_directories(.)
aux_source_directory(. RTKLIB_SRC)
aux_source_directory(rcv RTKLIB_RCV_SRC)

add_library(rtklib ${RTKLIB_SRC} ${RTKLIB_RCV_SRC})
# if link with some libraries
target_link_libraries(rtklib
    pthread
    ${LIBS}
)

