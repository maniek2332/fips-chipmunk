cmake_minimum_required(VERSION 2.8.5)

set(CHIPMUNK_VERSION_MAJOR 7)
set(CHIPMUNK_VERSION_MINOR 0)
set(CHIPMUNK_VERSION_PATCH 1)
set(CHIPMUNK_VERSION "${CHIPMUNK_VERSION_MAJOR}.${CHIPMUNK_VERSION_MINOR}.${CHIPMUNK_VERSION_PATCH}")
message("Configuring Chipmunk2D version ${CHIPMUNK_VERSION}")

fips_begin_sharedlib(chipmunk)

if(FIPS_MSVC)
    message(FATAL_ERROR "MSVC not supported")
endif()

if(NOT FIPS_MSVC)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu99") # always use gnu99
# TODO? -ffast-math (Release) and -Wall (Debug) flags
endif()

fips_include_directories(Chipmunk/include)

fips_files(
    Chipmunk/src/cpGearJoint.c
    Chipmunk/src/cpConstraint.c
    Chipmunk/src/chipmunk.c
    Chipmunk/src/cpGrooveJoint.c
    Chipmunk/src/cpRatchetJoint.c
    Chipmunk/src/cpBody.c
    Chipmunk/src/cpSpaceDebug.c
    Chipmunk/src/cpSpatialIndex.c
    Chipmunk/src/cpSpaceComponent.c
    Chipmunk/src/cpDampedSpring.c
    Chipmunk/src/cpSpace.c
    Chipmunk/src/cpSpaceStep.c
    Chipmunk/src/cpMarch.c
    Chipmunk/src/cpSweep1D.c
    Chipmunk/src/cpSlideJoint.c
    Chipmunk/src/cpRobust.c
    Chipmunk/src/cpBBTree.c
    Chipmunk/src/cpArray.c
    Chipmunk/src/cpPolyShape.c
    Chipmunk/src/cpSpaceHash.c
    Chipmunk/src/cpHashSet.c
    Chipmunk/src/cpPinJoint.c
    Chipmunk/src/cpArbiter.c
    Chipmunk/src/cpPivotJoint.c
    Chipmunk/src/cpPolyline.c
    Chipmunk/src/cpShape.c
    # Chipmunk/src/cpHastySpace.c  # requires sys/sysctl.h (Android)
    Chipmunk/src/cpSimpleMotor.c
    Chipmunk/src/cpRotaryLimitJoint.c
    Chipmunk/src/cpDampedRotarySpring.c
    Chipmunk/src/cpSpaceQuery.c
    Chipmunk/src/cpCollision.c
)

fips_end_sharedlib()

# need to explicitly link to the math library because the CMake/Android toolchains may not do it automatically
if(FIPS_ANDROID OR FIPS_POSIX)
    target_link_libraries(chipmunk m)
endif()
