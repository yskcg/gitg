bin_PROGRAMS += gitg/gitg

gitg_gitg_CPPFLAGS =						\
	-I$(top_srcdir)						\
	-I$(srcdir)						\
	-I$(top_srcdir)/libgd					\
	-DDATADIR=\""$(datadir)"\"				\
	-DGITG_DATADIR=\""$(datadir)/gitg"\"			\
	-DGITG_LOCALEDIR=\""$(datadir)/locale"\"

gitg_gitg_CFLAGS =						\
	$(GITG_CFLAGS)						\
	$(WARN_CFLAGS)						\
	-w

gitg_gitg_VALAFLAGS =						\
	--vapidir $(top_srcdir)/vapi				\
	$(GITG_VALAFLAGS)					\
	--pkg config						\
	--pkg ggit-1.0						\
	--pkg gtk+-3.0						\
	--pkg gio-2.0						\
	--pkg libpeas-1.0					\
	--pkg gobject-introspection-1.0				\
	--pkg gee-0.8						\
	--pkg gd-1.0						\
	--pkg $(WEBKIT_PKGCONFIG)				\
	--pkg gtksourceview-3.0					\
	--pkg GtkSpell-3.0					\
	--girdir "$(top_builddir)/libgd"			\
	--girdir "$(top_builddir)"				\
	--vapidir "$(top_srcdir)/vapi"				\
	--vapidir "$(top_builddir)/libgd"			\
	--vapidir "$(top_builddir)/libgitg"			\
	--vapidir "$(top_builddir)/libgitg-ext"			\
	--gresources "$(top_srcdir)/gitg/resources/gitg-resources.xml"

gitg_gitg_LDADD =						\
	libgd/libgd.la						\
	libgitg/libgitg-1.0.la					\
	libgitg-ext/libgitg-ext-1.0.la

gitg_gitg_LDFLAGS =						\
	-export-dynamic						\
	-no-undefined						\
	-export-symbols-regex "^[[^_]].*"			\
	$(GITG_LIBS)						\
	$(PACKAGE_LIBS)

gitg_gitg_VALASOURCES =						\
	gitg/gitg.vala						\
	gitg/gitg-dirs.vala					\
	gitg/gitg-window.vala					\
	gitg/gitg-clone-dialog.vala				\
	gitg/gitg-author-details-dialog.vala			\
	gitg/gitg-resource.vala					\
	gitg/gitg-dash-view.vala				\
	gitg/gitg-application.vala				\
	gitg/gitg-plugins-engine.vala				\
	gitg/gitg-popup-menu.vala				\
	gitg/gitg-animated-paned.vala				\
	gitg/gitg-ui-elements.vala				\
	gitg/gitg-ref-action-rename.vala			\
	gitg/gitg-ref-action-delete.vala			\
	gitg/gitg-ref-action-copy-name.vala			\
	gitg/gitg-ref-action-fetch.vala				\
	gitg/gitg-commit-action-create-branch.vala		\
	gitg/gitg-create-branch-dialog.vala			\
	gitg/gitg-commit-action-create-tag.vala			\
	gitg/gitg-create-tag-dialog.vala			\
	gitg/gitg-commit-action-create-patch.vala		\
	gitg/gitg-remote-manager.vala				\
	gitg/preferences/gitg-preferences-commit.vala		\
	gitg/preferences/gitg-preferences-dialog.vala		\
	gitg/preferences/gitg-preferences-interface.vala	\
	gitg/preferences/gitg-preferences-history.vala		\
	gitg/history/gitg-history.vala				\
	gitg/history/gitg-history-action-interface.vala		\
	gitg/history/gitg-history-refs-list.vala		\
	gitg/history/gitg-history-paned.vala			\
	gitg/commit/gitg-commit.vala				\
	gitg/commit/gitg-commit-paned.vala			\
	gitg/commit/gitg-commit-sidebar.vala			\
	gitg/commit/gitg-commit-submodule-history-view.vala	\
	gitg/commit/gitg-commit-submodule-diff-view.vala	\
	gitg/commit/gitg-commit-submodule-info.vala		\
	gitg/commit/gitg-commit-dialog.vala			\
	libgitg/libgitg-1.0.vapi				\
	libgitg-ext/libgitg-ext-1.0.vapi

gitg_gitg_SOURCES =						\
	$(gitg_gitg_VALASOURCES)				\
	gitg/gitg-resources.c

gitg_resources_deps = $(call GRESDEPS,gitg/resources/gitg-resources.xml)
gitg/gitg-resources.c: $(gitg_resources_deps)
	$(GRESGEN)

CLEANFILES += gitg/gitg-resources.c
EXTRA_DIST += $(gitg_resources_deps)

# vi:ts=8:noet
