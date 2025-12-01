/* See LICENSE file for copyright and license details. */

/* appearance */
static const unsigned int borderpx  = 1;        /* border pixel of windows */
static const unsigned int snap      = 32;       /* snap pixel */
static const unsigned int gappih    = 20;       /* horiz inner gap between windows */
static const unsigned int gappiv    = 20;       /* vert inner gap between windows */
static const unsigned int gappoh    = 20;       /* horiz outer gap between windows and screen edge */
static const unsigned int gappov    = 30;       /* vert outer gap between windows and screen edge */
static       int smartgaps          = 0;        /* 1 means no outer gap when there is only one window */
static const int showbar            = 1;        /* 0 means no bar */
static const int topbar             = 1;        /* 0 means bottom bar */
static const char *fonts[]          = { "JetBrains Mono NerdFont:size=14", "Noto Sans CJK JP:size=14" };
static const char dmenufont[]       = "JetBrains Mono NerdFont:size=14";
static char normbgcolor[]           = "#222222";
static char normbordercolor[]       = "#444444";
static char normfgcolor[]           = "#bbbbbb";
static char selfgcolor[]            = "#eeeeee";
static char selbordercolor[]        = "#005577";
static char selbgcolor[]            = "#005577";
static char *colors[][3] = {
       /*               fg           bg           border   */
       [SchemeNorm] = { normfgcolor, normbgcolor, normbordercolor },
       [SchemeSel]  = { selfgcolor,  selbgcolor,  selbordercolor  },
	/* for bar --> {text, background, null}  */
	[SchemeStatus]  = { normfgcolor, normbgcolor,  normbgcolor  }, // Statusbar right {text,background,not used but cannot be empty}
	[SchemeTagsSel]  = { selfgcolor, selbgcolor,  normbgcolor  }, // Tagbar left selected {text,background,not used but cannot be empty}
	[SchemeTagsNorm]  = { normfgcolor, normbgcolor,  normbgcolor  }, // Tagbar left unselected {text,background,not used but cannot be empty}
	[SchemeInfoSel]  = { selfgcolor, selbgcolor,  normbgcolor  }, // infobar middle  selected {text,background,not used but cannot be empty}
	[SchemeInfoNorm]  = { normfgcolor, normbgcolor,  normbgcolor  }, // infobar middle  unselected {text,background,not used but cannot be empty}
};

/* tagging */
static const char *tags[] = { "1", "2", "3", "4", "5", "6", "7", "8", "9" };

static const Rule rules[] = {
	/* xprop(1):
	 *	WM_CLASS(STRING) = instance, class
	 *	WM_NAME(STRING) = title
	 */
	/* class      instance    title       tags mask     isfloating   monitor */
	{ "Gimp",     NULL,       NULL,       0,            1,           -1 },
	{ "Firefox",  NULL,       NULL,       1 << 8,       0,           -1 },
	{ "Tk",       NULL,       NULL,       0,            1,           -1 },
};

/* layout(s) */
static const float mfact     = 0.55; /* factor of master area size [0.05..0.95] */
static const int nmaster     = 1;    /* number of clients in master area */
static const int resizehints = 1;    /* 1 means respect size hints in tiled resizals */
static const int lockfullscreen = 1; /* 1 will force focus on the fullscreen window */
static const int refreshrate = 120;  /* refresh rate (per second) for client move/resize */

#define FORCE_VSPLIT 1  /* nrowgrid layout: force two clients to always split vertically */
#include "vanitygaps.c"
#include <X11/XF86keysym.h>
#include "movestack.c"
static const Layout layouts[] = {
	/* symbol     arrange function */
	{ "󰓒",      tile },    /* first entry is default */
	{ "󰇥",      NULL },    /* no layout function means floating behavior */
	{ "",      monocle },
	{ "󰫣",      spiral },
	{ "󰫥",      dwindle },
};

/* key definitions */
#define MODKEY Mod4Mask // windows key
#define TAGKEYS(KEY,TAG) \
	{ MODKEY,                       KEY,      view,           {.ui = 1 << TAG} }, \
	{ MODKEY|ControlMask,           KEY,      toggleview,     {.ui = 1 << TAG} }, \
	{ MODKEY|ShiftMask,             KEY,      tag,            {.ui = 1 << TAG} }, \
	{ MODKEY|ControlMask|ShiftMask, KEY,      toggletag,      {.ui = 1 << TAG} },

/* helper for spawning shell commands in the pre dwm-5.0 fashion */
#define SHCMD(cmd) { .v = (const char*[]){ "/bin/sh", "-c", cmd, NULL } }

#define STATUSBAR "dwmblocks"
#define BROWSER "firefox"

/* commands */
static char dmenumon[2] = "0"; /* component of dmenucmd, manipulated in spawn() */
static const char *dmenucmd[] = { "dmenu_run", "-fn", dmenufont, "-nb", normbgcolor, "-nf", normfgcolor, "-sb", selbordercolor, "-sf", selfgcolor, NULL };
static const char *termcmd[]  = { "st", NULL };

/* czardien made it work with @DEFAULT_AUDIO_SINK@ */
static const char *upvol[]      = { "/usr/bin/wpctl",   "set-volume", "@DEFAULT_AUDIO_SINK@",      "5%+",      NULL };
static const char *downvol[]    = { "/usr/bin/wpctl",   "set-volume", "@DEFAULT_AUDIO_SINK@",      "5%-",      NULL };
static const char *mutevol[]    = { "/usr/bin/wpctl",   "set-mute",   "@DEFAULT_AUDIO_SINK@",      "toggle",   NULL };

/* brightness stuff */
static const char *brupcmd[] = { "brightnessctl", "set", "10%+", NULL };
static const char *brdowncmd[] = { "brightnessctl", "set", "10%-", NULL };
static const char *prtscrcmd[] = { "flameshot", "gui", NULL};


static const Key keys[] = {
	/* modifier                     key        function        argument */
	{ MODKEY,                       XK_d,      spawn,          {.v = dmenucmd } },
	{ MODKEY,	                XK_Return, spawn,          {.v = termcmd } },
	{ MODKEY,                       XK_b,      togglebar,      {0} },
	{ MODKEY,                       XK_j,      focusstack,     {.i = +1 } },
	{ MODKEY,                       XK_k,      focusstack,     {.i = -1 } },
	{ MODKEY,                       XK_z,      incnmaster,     {.i = +1 } },
	{ MODKEY,                       XK_x,      incnmaster,     {.i = -1 } },
	{ MODKEY,                       XK_h,      setmfact,       {.f = -0.05} },
	{ MODKEY,                       XK_l,      setmfact,       {.f = +0.05} },
	{ MODKEY|ShiftMask,             XK_h,      setcfact,       {.f = +0.25} },
	{ MODKEY|ShiftMask,             XK_l,      setcfact,       {.f = -0.25} },
	{ MODKEY|ShiftMask,             XK_o,      setcfact,       {.f =  0.00} },
	{ MODKEY|ShiftMask,             XK_j,      movestack,      {.i = +1 } },
	{ MODKEY|ShiftMask,             XK_k,      movestack,      {.i = -1 } },	
	{ MODKEY,                       XK_Return, zoom,           {0} },

	{ MODKEY,                       XK_Tab,    view,           {0} },
	{ MODKEY, 		        XK_c,      killclient,     {0} },

	{ MODKEY,                       XK_t,      setlayout,      {.v = &layouts[0]} },
	{ MODKEY,                       XK_f,      setlayout,      {.v = &layouts[1]} },
	{ MODKEY,                       XK_m,      setlayout,      {.v = &layouts[2]} },
	{ MODKEY,                       XK_s,      setlayout,      {.v = &layouts[3]} },
	{ MODKEY,                       XK_a,      setlayout,      {.v = &layouts[4]} },
	{ MODKEY,                       XK_space,  setlayout,      {0} },
	{ MODKEY|ShiftMask,             XK_space,  togglefloating, {0} },
	{ MODKEY,                       XK_0,      view,           {.ui = ~0 } },
	{ MODKEY|ShiftMask,             XK_0,      tag,            {.ui = ~0 } },
	{ MODKEY,                       XK_comma,  focusmon,       {.i = -1 } },
	{ MODKEY,                       XK_period, focusmon,       {.i = +1 } },
	{ MODKEY|ShiftMask,             XK_comma,  tagmon,         {.i = -1 } },
	{ MODKEY|ShiftMask,             XK_period, tagmon,         {.i = +1 } },
	{ MODKEY,                       XK_w,      xrdb,           {.v = NULL } },
	{ MODKEY|ShiftMask,             XK_q,      quit,           {0} },
	{ MODKEY|ControlMask|ShiftMask, XK_q,      quit,           {1} },


/* gaps control */	
	{ MODKEY,                       XK_minus,  incrgaps,       {.i = -3 } },
	{ MODKEY,                       XK_equal,  incrgaps,       {.i = +3 } },
	{ MODKEY|Mod1Mask,              XK_i,      incrigaps,      {.i = +1 } },
	{ MODKEY|Mod1Mask|ShiftMask,    XK_i,      incrigaps,      {.i = -1 } },
	{ MODKEY|Mod1Mask,              XK_o,      incrogaps,      {.i = +1 } },
	{ MODKEY|Mod1Mask|ShiftMask,    XK_o,      incrogaps,      {.i = -1 } },
	{ MODKEY|Mod1Mask,              XK_6,      incrihgaps,     {.i = +1 } },
	{ MODKEY|Mod1Mask|ShiftMask,    XK_6,      incrihgaps,     {.i = -1 } },
	{ MODKEY|Mod1Mask,              XK_7,      incrivgaps,     {.i = +1 } },
	{ MODKEY|Mod1Mask|ShiftMask,    XK_7,      incrivgaps,     {.i = -1 } },
	{ MODKEY|Mod1Mask,              XK_8,      incrohgaps,     {.i = +1 } },
	{ MODKEY|Mod1Mask|ShiftMask,    XK_8,      incrohgaps,     {.i = -1 } },
	{ MODKEY|Mod1Mask,              XK_9,      incrovgaps,     {.i = +1 } },
	{ MODKEY|Mod1Mask|ShiftMask,    XK_9,      incrovgaps,     {.i = -1 } },
	{ MODKEY|ShiftMask,             XK_equal,  togglegaps,     {0} },
	{ MODKEY|ShiftMask,             XK_minus,  defaultgaps,    {0} },

/* tag keys */
	TAGKEYS(                        XK_1,                      0)
	TAGKEYS(                        XK_2,                      1)
	TAGKEYS(                        XK_3,                      2)
	TAGKEYS(                        XK_4,                      3)
	TAGKEYS(                        XK_5,                      4)
	TAGKEYS(                        XK_6,                      5)
	TAGKEYS(                        XK_7,                      6)
	TAGKEYS(                        XK_8,                      7)
	TAGKEYS(                        XK_9,                      8)

/* Application  Bindings */

	{ MODKEY,			XK_r,      spawn,       {.v = (const char*[]){ "st", "-e", "ranger",  NULL } } },
	{ MODKEY,			XK_e,      spawn,       {.v = (const char*[]){ BROWSER,  NULL } } },

/* Script Launch Bindings */

	{ Mod1Mask,			XK_b,      spawn,       {.v = (const char*[]){ "setbg", NULL } } },
	{ Mod1Mask,			XK_s,      spawn,       {.v = (const char*[]){ "sys", NULL } } },


/* Other Bindings  */

	{ 0,                            XF86XK_AudioLowerVolume,     spawn, {.v = downvol } },
	{ 0,                            XF86XK_AudioMute,            spawn, {.v = mutevol } },
	{ 0,                            XF86XK_AudioRaiseVolume,     spawn, {.v = upvol   } },
	{ 0,                            XF86XK_MonBrightnessUp,      spawn, {.v = brupcmd } },
    	{ 0,                            XF86XK_MonBrightnessDown,    spawn, {.v = brdowncmd } }, 
    	{ 0,                            XK_Print,                    spawn, {.v = prtscrcmd } },

};

/* button definitions */
/* click can be ClkTagBar, ClkLtSymbol, ClkStatusText, ClkWinTitle, ClkClientWin, or ClkRootWin */
static const Button buttons[] = {
	/* click                event mask      button          function        argument */
#ifndef __OpenBSD__
	{ ClkWinTitle,          0,		Button2,	zoom,           {0} },
	{ ClkStatusText,        0,		Button1,	sigstatusbar,   {.i = 1} },
	{ ClkStatusText,        0,		Button2,	sigstatusbar,   {.i = 2} },
	{ ClkStatusText,        0,		Button3,	sigstatusbar,   {.i = 3} },
	{ ClkStatusText,        0,		Button4,	sigstatusbar,   {.i = 4} },
	{ ClkStatusText,        0,		Button5,	sigstatusbar,   {.i = 5} },
	{ ClkStatusText,	ShiftMask,	Button1,	sigstatusbar,	{.i = 6} },
#endif
	{ ClkLtSymbol,          0,              Button1,        setlayout,      {0} },
	{ ClkLtSymbol,          0,              Button3,        setlayout,      {.v = &layouts[2]} },
	{ ClkWinTitle,          0,              Button2,        zoom,           {0} },
	{ ClkStatusText,        0,              Button1,        sigstatusbar,   {.i = 1} },
	{ ClkStatusText,        0,              Button2,        sigstatusbar,   {.i = 2} },
	{ ClkStatusText,        0,              Button3,        sigstatusbar,   {.i = 3} },
	{ ClkClientWin,         MODKEY,         Button1,        movemouse,      {0} },
	{ ClkClientWin,         MODKEY,         Button2,        togglefloating, {0} },
	{ ClkClientWin,         MODKEY,         Button3,        resizemouse,    {0} },
	{ ClkTagBar,            0,              Button1,        view,           {0} },
	{ ClkTagBar,            0,              Button3,        toggleview,     {0} },
	{ ClkTagBar,            MODKEY,         Button1,        tag,            {0} },
	{ ClkTagBar,            MODKEY,         Button3,        toggletag,      {0} },
};
