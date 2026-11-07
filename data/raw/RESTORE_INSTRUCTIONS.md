This archive preserves scientific manifests, raw artifacts, executed code and calibration snapshots.
It deliberately omits derived per-stage ZIP files. In the master notebook, run the definition cells, then call
restored_store = restore_run_export(archive_path, empty_output_directory).
Restoration verifies every checksum and rebuilds derived ZIPs in dependency order without running experiments.
Set the notebook output directory to that directory before continuing. Completion pointers retain the original
pointer and restoration provenance; scientific manifest hashes and original execution IDs remain unchanged.
