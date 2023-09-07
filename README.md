# Neorg Roam

- Trying to do some of the things that org-roam does.

## Install

Install with packer
```lua 
use({"jarvismkennedy/neorgroam.nvim", 
 requires = { 
  "nvim-telescope/telescope.nvim", 
  "nvim-lua/plenary.nvim"
 }
})
```
  And then set it up as a neorg module.
```lua
require("neorg").setup({ 
 load = { 
  ["core.defaults"] = {},
  ["core.dirman"] = {
   config = { 
	   workspaces = { 
		   notes = "~/Documents/neorg/notes"
		   roam = "~/Documents/neorg/roam"
	   }
	   default_workspace = "roam"
   }
  },
  ["core.integrations.roam"] = { 
   -- default keymaps
   config  = {
	   keymaps = {
		   -- select_prompt is used to to create new note / capture from the prompt directly
		   -- instead of the telescope choice.
		   select_prompt = "<C-n>",
		   insert_link = "<leader>ni",
		   find_note = "<leader>nf",
		   capture_note = "<leader>nc",
		   capture_index = "<leader>nci",
		   capture_cancel = "<C-q>",
		   capture_save = "<C-w>",
	   },
	   -- telescope theme
	   theme = "ivy",

	   capture_templates = {
		   {
			   name = "default",
			   file =  "${title}",
			   narrowed = false,
			   target = "metadata",
			   lines = { "" }, 
		   }
	   },
	   substitutions = {
		   title = function(metadata)
			   return metadata.title
		   end,
		   date = function(metadata)
			   return os.date("%Y-%m-%d")
		   end
	   }
   }
  }
 }
})
```


## Capture templates

Capture templates are defined as a list in the roam config table.
```lua	
{
 name = "CS new note",
 file = "classes/computer_science/${class}/${title}_${date}",
 lines = { "","* ${class} Lecture ${date}" },
}
```
Capture templates have support for substitution. Substitutions are functions defined in the
config table which take the metadata table as a parameter and  return a string. The default
substitutions functions are shown in the default config above. If a substitution function is not
found then you will be prompted for the value.


### Capture templates to do

-  Currently capture templates always insert after the metadata. Support a target property to
      insert after any treesitter node.
-  Support the `narrowed` flag to capture in a blank buffer and write lines to file on save.
-  Don't open a floating capture window if the file is in the current buffer. 



## currently implemented features

- Find notes.
- Insert links to norg files.
- Capture notes. 
- Capture to the index file.
- All of these methods create the files if they don't exist.
- Capture templates


## TODO:

- Create sql module.
- Implement back links.
- Other types of linkables.
