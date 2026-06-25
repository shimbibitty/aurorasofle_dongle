if(CONFIG_ZMK_KEYBOARD_NAME STREQUAL "" OR SHIELD MATCHES "sofle_dongle")
  # Nothing extra to compile for the dongle — it's a pure central relay.
endif()
