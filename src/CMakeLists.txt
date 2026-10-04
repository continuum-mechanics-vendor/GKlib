
set(win32_inc $<$<BOOL:${MSVC}>:${PROJECT_SOURCE_DIR}/win32>)
set(win32_sources $<$<BOOL:${MSVC}>:win32/adapt.c>)

set(GKlib_sources b64.c         evaluate.c      gkregex.c       mcore.c         seq.c
blas.c          fkvkselect.c    graph.c         memory.c        sort.c
cache.c         fs.c            htable.c        pqueue.c        string.c
csr.c           getopt.c        io.c            random.c        timers.c
error.c         gk_util.c       itemsets.c      rw.c            tokenizer.c)

add_library(GKlib ${GKlib_sources} ${win32_sources})
target_include_directories(GKlib PUBLIC
  $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)
target_link_libraries(GKlib PRIVATE
  $<$<BOOL:${UNIX}>:m>
  $<$<BOOL:${OPENMP}>:OpenMP::OpenMP_C>
)

add_library(GKlib::GKlib ALIAS GKlib)

