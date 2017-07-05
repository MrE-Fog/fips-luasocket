if (NOT FIPS_IMPORT)
    cmake_minimum_required(VERSION 2.8)
    get_filename_component(FIPS_ROOT_DIR "../fips" ABSOLUTE)
    include("${FIPS_ROOT_DIR}/cmake/fips.cmake")
    fips_setup(PROJECT fips-luasocket)
endif()

if (FIPS_WINDOWS OR FIPS_OSX OR FIPS_LINUX)
    fips_begin_lib(luasocket)
        fips_dir(luasocket/src)
        fips_files(
            auxiliar.c auxiliar.h
            buffer.c buffer.h
            compat.c compat.h
            except.c except.h
            inet.c inet.h
            io.c io.h
            luasocket.c luasocket.h
            options.c options.h
            pierror.h
            select.c select.h
            socket.h
            tcp.c tcp.h
            timeout.c timeout.h
            udp.c udp.h
        )
        if (FIPS_WINDOWS)
            fips_files(wsocket.c wsocket.h)
        else()
            fips_files(
                unix.c unix.h 
                unixdgram.c unixdgram.h 
                unixstream.c unixstream.h
                usocket.c usocket.h
            )
        endif()
        fips_deps(lua)
    fips_end_lib()
    target_compile_definitions(luasocket PRIVATE LUASOCKET_API=)
endif()

if (NOT FIPS_IMPORT)
    fips_finish()
endif()


