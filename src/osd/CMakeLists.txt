set(osdc_osd_srcs
  ${CMAKE_SOURCE_DIR}/src/osdc/Objecter.cc
  ${CMAKE_SOURCE_DIR}/src/osdc/Striper.cc)

set(osd_srcs
  OSD.cc
  Watch.cc
  ClassHandler.cc
  OpRequest.cc
  PG.cc
  PGLog.cc
  PrimaryLogPG.cc
  ReplicatedBackend.cc
  ECBackend.cc
  ECTransaction.cc
  PGBackend.cc
  OSDCap.cc
  Watch.cc
  ClassHandler.cc
  SnapMapper.cc
  ScrubStore.cc
  osd_types.cc
  ECUtil.cc
  ExtentCache.cc
  /home/obel/inkpackdir/secret_split_and_rebuild.h
  /home/obel/inkpackdir/secret_split_and_rebuild.cpp
  /home/obel/inkpackdir/encryption.h
  /home/obel/inkpackdir/encryption.cpp
  ${CMAKE_SOURCE_DIR}/src/common/TrackedOp.cc
  ${osdc_osd_srcs})
if(HAS_VTA)
  set_source_files_properties(osdcap.cc
    PROPERTIES COMPILE_FLAGS -fno-var-tracking-assignments)
endif()
add_library(osd STATIC ${osd_srcs}
  $<TARGET_OBJECTS:osd_mon_objs>
  $<TARGET_OBJECTS:cls_references_objs>
  $<TARGET_OBJECTS:global_common_objs>
  $<TARGET_OBJECTS:heap_profiler_objs>
  $<TARGET_OBJECTS:common_util_obj>)
target_link_libraries(osd /home/obel/inkpackdir/intel_aes_lib/lib/x64/intel_aes64.a)
target_link_libraries(osd ${LEVELDB_LIBRARIES} ${CMAKE_DL_LIBS} ${ALLOC_LIBS})
#FIND_FILE(gferasure libgferasure.so ./home/obel/gferasure/src/)
#secret_split_and_rebuild encryption
target_link_libraries(osd /home/obel/gferasure/src/libgferasure.so)
if(WITH_LTTNG)
  add_dependencies(osd oprequest-tp osd-tp pg-tp)
endif()
