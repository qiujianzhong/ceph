set(rbd_mirror_test_srcs
  test_ClusterWatcher.cc
  test_PoolWatcher.cc
  test_ImageReplayer.cc
  test_ImageDeleter.cc
  test_ImageSync.cc
  test_LeaderWatcher.cc
  test_fixture.cc
  )
add_library(rbd_mirror STATIC ${rbd_mirror_test_srcs})
set_target_properties(rbd_mirror PROPERTIES COMPILE_FLAGS
  ${UNITTEST_CXX_FLAGS})

add_executable(unittest_rbd_mirror
  test_main.cc
  test_mock_fixture.cc
  test_mock_ImageReplayer.cc
  test_mock_ImageSync.cc
  test_mock_ImageSyncThrottler.cc
  test_mock_LeaderWatcher.cc
  image_replayer/test_mock_BootstrapRequest.cc
  image_replayer/test_mock_CreateImageRequest.cc
  image_replayer/test_mock_EventPreprocessor.cc
  image_sync/test_mock_ImageCopyRequest.cc
  image_sync/test_mock_ObjectCopyRequest.cc
  image_sync/test_mock_SnapshotCopyRequest.cc
  image_sync/test_mock_SnapshotCreateRequest.cc
  image_sync/test_mock_SyncPointCreateRequest.cc
  image_sync/test_mock_SyncPointPruneRequest.cc
  )
add_ceph_unittest(unittest_rbd_mirror ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/unittest_rbd_mirror)
set_target_properties(unittest_rbd_mirror PROPERTIES COMPILE_FLAGS
  ${UNITTEST_CXX_FLAGS})
add_dependencies(unittest_rbd_mirror
  cls_journal
  cls_lock
  cls_rbd)
target_link_libraries(unittest_rbd_mirror
  rbd_mirror
  rados_test_stub
  rbd_mirror_internal
  rbd_mirror_types
  rbd_api
  rbd_internal
  rbd_test_mock
  journal
  journal_test_mock
  cls_rbd_client
  cls_lock_client
  cls_journal_client
  rbd_types
  librados
  osdc
  global
  radostest
  )

add_executable(ceph_test_rbd_mirror
  test_main.cc
  )
set_target_properties(ceph_test_rbd_mirror PROPERTIES COMPILE_FLAGS
  ${UNITTEST_CXX_FLAGS})
target_link_libraries(ceph_test_rbd_mirror
  rbd_mirror
  rbd_mirror_internal
  rbd_mirror_types
  rbd_api
  rbd_internal
  journal
  cls_rbd_client
  cls_journal_client
  rbd_types
  rados_a
  radostest
  ${UNITTEST_LIBS}
  )

add_executable(ceph_test_rbd_mirror_random_write
  random_write.cc)
set_target_properties(ceph_test_rbd_mirror_random_write PROPERTIES COMPILE_FLAGS
  ${UNITTEST_CXX_FLAGS})
target_link_libraries(ceph_test_rbd_mirror_random_write
  librbd librados global)

install(TARGETS
  ceph_test_rbd_mirror
  ceph_test_rbd_mirror_random_write
  DESTINATION ${CMAKE_INSTALL_BINDIR})
