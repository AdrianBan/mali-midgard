# mali-midgard
This is a personal effort to port Mali Midgard T6xx (T604, T658, T622, T624, T628, T676), T7xx (T720, T760, T820, T830, T860, T880) DKMS drivers to Debian Buster, based on Mali 16.0 Debian packages and updated to r28p0-01rel0 + patches to fix compilation on newer Kernels.

## Porting
I'm trying to fix the Mali OpenSource code to be ported to newer kernel versions (5.0 and up).
Issues I've faced:
- function `void *dma_buf_kmap(struct dma_buf *dmabuf, unsigned long page_num)` and `void dma_buf_kunmap(struct dma_buf *, unsigned long, void *);` were removed from Kernel 5.6. So I had to move the code to dma_buf_vmap and dma_buf_vunmap. Hopt that it will work.
- fixing a lot of `case` statements to avoid `warning: this statement may fall through`. The original code had missing `brake` in the code in `backend/gpu/mali_kbase_jm_rb.c`. Hopping that is not affecting the driver :). Looking into the code I've notice that falling trhough a next statemane actually was rewriting 
- fixing `snprintf(str, size, "%u", fence->seqno);` to `snprintf(str, size, "%lu", fence->seqno);` in `mali_kbase_fence.c`. The `fence->seqno` now is using u64 instead `unsigned`.
- moved from `vm_insert_pfn` function to `vmf_insert_pfn` in `mali_kbase_mem_linux.c`. This function was renamed starting from Kernel 4.20

