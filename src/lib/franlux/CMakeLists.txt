# ==============================================================================
# RANLUX CMake File
# ==============================================================================

# objects in library -----------------------------------------------------------
SET(FRANLUX_OBJ
        acpr_v1_0.f
        )

# create static library --------------------------------------------------------
IF(LIBS_STATIC)
    ADD_LIBRARY(franlux_static STATIC ${FRANLUX_OBJ})
    SET_TARGET_PROPERTIES(franlux_static PROPERTIES
                            OUTPUT_NAME franlux
                            CLEAN_DIRECT_OUTPUT 1)
ENDIF(LIBS_STATIC)

# create shared library --------------------------------------------------------
IF(LIBS_SHARED)
    ADD_LIBRARY(franlux_shared SHARED ${FRANLUX_OBJ})
    SET_TARGET_PROPERTIES(franlux_shared PROPERTIES
                            OUTPUT_NAME franlux
                            CLEAN_DIRECT_OUTPUT 1
                            VERSION ${RANLUX_VERSION})
    INSTALL(TARGETS franlux_shared
            DESTINATION lib)
ENDIF(LIBS_SHARED)


