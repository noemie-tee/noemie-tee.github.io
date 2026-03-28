---
title: "Asset Validation Tool : Renaming and Relocating"
excerpt: "<img src='/images/asset%20validation%20tool.png' width=50 height=50>Unreal right-click tool that renames assets and place them into relevant folders. "
collection: portfolio
---

The script is accessible as a Scripted Asset Action by right-clicking in one or several assets in the Content Browser.

```Python
import unreal

EUL = unreal.EditorUtilityLibrary
EAL = unreal.EditorAssetLibrary

selected_assets = EUL.get_selected_assets()

def rename_and_relocate_assets(selected_assets) :
        assettools = unreal.AssetToolsHelpers.get_asset_tools()

# checking the required folders exist, creating them otherwise

        SM_dir_path = "/Game/Art/StaticMeshes"
        MM_dir_path = "/Game/Art/Materials/Masters"
        MI_dir_path = "/Game/Art/Materials/Instances"
        T_dir_path = "/Game/Art/Textures"


        if not EAL.does_directory_exist(SM_dir_path):
            EAL.make_directory(SM_dir_path)


        if not EAL.does_directory_exist(MM_dir_path):
            EAL.make_directory(MM_dir_path)


        if not EAL.does_directory_exist(MI_dir_path):
            EAL.make_directory(MI_dir_path)


        if not EAL.does_directory_exist(T_dir_path):
            EAL.make_directory(T_dir_path)




        rename_data_list = []
        for asset in selected_assets:

            # extracting the initial name of the asset
            previous_asset_name = asset.get_name()
            previous_subfolder_temp = asset.get_path_name().replace("/Game","")
            previous_subfolder = previous_subfolder_temp.rsplit('/')[0]
           

            # building the new name and path for the asset
            if isinstance(asset, unreal.StaticMesh):
                new_path = SM_dir_path + "/" + previous_subfolder
                new_name = previous_asset_name

                if not new_name.startswith("SM_"):
                    new_name = "SM_" + previous_asset_name
                else:
                    pass

               # storing the naming and relocation data in a list
                NewAssetData = unreal.AssetRenameData(asset, new_path, new_name)

                rename_data_list.append(NewAssetData)
                unreal.log(f"Renaming {previous_asset_name} to {new_name}.")

            elif isinstance(asset, unreal.Material):

                new_path = MM_dir_path + "/" + previous_subfolder
                new_name = previous_asset_name

                if not new_name.startswith("MM_"):
                    new_name = "MM_" + previous_asset_name
                else:
                    pass

                NewAssetData = unreal.AssetRenameData(asset, new_path, new_name)

                rename_data_list.append(NewAssetData)
                unreal.log(f"Renaming {previous_asset_name} to {new_name}.")

            elif isinstance(asset, unreal.MaterialInstance):

                new_path = MI_dir_path + "/" + previous_subfolder
                new_name = previous_asset_name

                if not new_name.startswith("MI_"):
                    new_name = "MI_" + previous_asset_name
                else:
                    pass

                NewAssetData = unreal.AssetRenameData(asset, new_path, new_name)

                rename_data_list.append(NewAssetData)
                unreal.log(f"Renaming {previous_asset_name} to {new_name}.")

            elif isinstance(asset, unreal.Texture):

                new_path = T_dir_path + "/" + previous_subfolder
                new_name = previous_asset_name

                if not new_name.startswith("T_"):
                    new_name = "T_" + previous_asset_name
                else:
                    pass

                NewAssetData = unreal.AssetRenameData(asset, new_path, new_name)

                rename_data_list.append(NewAssetData)
                unreal.log(f"Renaming {previous_asset_name} to {new_name}.")

        # batch renaming and relocating from the previous list
        if rename_data_list :
            assettools.rename_assets(rename_data_list)
        print("successfully renamed assets")


rename_and_relocate_assets(selected_assets)
```
