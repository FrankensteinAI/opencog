# Pattern miner build file.
ADD_SUBDIRECTORY (types)

IF (HAVE_cpprest)
    ADD_LIBRARY (PatternMiner SHARED
            HTree
            Parameters
            PatternMiner
            PatternMinerBF
            PatternMinerDF
            PatternMinerDistributedWorker
            PatternMinerCentralServer
    )

    TARGET_LINK_LIBRARIES (PatternMiner
        ${COGUTIL_LIBRARY}
        ${ATOMSPACE_LIBRARIES}
        patternminer-types
        ${cpprest_LIBRARY}
        ${Boost_SYSTEM_LIBRARY}
        ${Boost_REGEX_LIBRARY}
    )
ELSE (HAVE_cpprest)
    ADD_LIBRARY (PatternMiner SHARED
            HTree
            Parameters
            PatternMiner
            PatternMinerBF
            PatternMinerDF
    )

    TARGET_LINK_LIBRARIES (PatternMiner
        ${COGUTIL_LIBRARY}
        ${ATOMSPACE_LIBRARIES}
        patternminer-types
        ${Boost_SYSTEM_LIBRARY}
        ${Boost_REGEX_LIBRARY}
    )
ENDIF (HAVE_cpprest)

ADD_DEPENDENCIES(PatternMiner spacetime_atom_types)

ADD_LIBRARY(PatternMinerAgent SHARED
	PatternMinerAgent
)


TARGET_LINK_LIBRARIES (PatternMinerAgent
	PatternMiner
)

INSTALL (FILES
	HTree.h
    Parameters.h
	PatternMiner.h
	DESTINATION "include/${PROJECT_NAME}/learning/PatternMiner"
)

INSTALL (TARGETS PatternMiner DESTINATION "lib${LIB_DIR_SUFFIX}/opencog")

IF (HAVE_cpprest)

    ADD_LIBRARY(DistributedPatternMinerClient SHARED
            DistributedPatternMinerClient
    )

    ADD_LIBRARY(DistributedPatternMinerServer SHARED
            DistributedPatternMinerServer
    )

    TARGET_LINK_LIBRARIES (DistributedPatternMinerClient
            PatternMiner
    )

    TARGET_LINK_LIBRARIES (DistributedPatternMinerServer
            PatternMiner
    )

    INSTALL (FILES
            DistributedPatternMiner.h
            DESTINATION "include/${PROJECT_NAME}/learning/PatternMiner"
    )

ENDIF (HAVE_cpprest)

# This defines the guile scheme loadable module

IF (HAVE_GUILE)
        ADD_LIBRARY (guile-patternminer SHARED
                PatternMinerSCM
        )

        TARGET_LINK_LIBRARIES(guile-patternminer
                PatternMiner
                ${GUILE_LIBRARIES}
        )

        INSTALL (TARGETS guile-patternminer DESTINATION "lib${LIB_DIR_SUFFIX}/opencog")

ENDIF (HAVE_GUILE)

# ------------------------------------------------------------
