<script lang="ts">
  import DB from "./db";
  import Block from "./editor/Block.svelte";
  import dayjs from "dayjs";
  import router from "./router";
  import rules from "./rules";

  /**
   * content: string
   * id: string
   * pageId: string
   * references: string[]
   */
  export let blockBody: Block;

  /**
   * block: block item in page
   * page: Page
   * position: number[]
   */
  let block;
  (() => {
    const blocksMap = DB.get().getFlatBlocksFromPage(blockBody.pageId);

    block = JSON.parse(JSON.stringify(blocksMap[blockBody.id]));
  })();
</script>

<div>
  <div class="mb-2 bg-blue-50 px-2 py-1 font-medium">
    <a
      on:click={(e) => {
        e.preventDefault();
        router.navigate(`/page/${block.page.id}`);
      }}
      href={`/page/${block.page.id}`}
      >{block.page.type === "daily"
        ? dayjs(block.page.title).format("MMMM, DD, YYYY")
        : block.page.title}</a
    >
  </div>

  <div>
    <Block
      {rules}
      editable={false}
      block={block.block}
      root={block.page}
      path={block.position}
    />
  </div>
</div>
