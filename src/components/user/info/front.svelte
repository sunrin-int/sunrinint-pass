<script lang="ts">
  import { onMount } from "svelte";
  import Background from "../../../assets/images/pass_background.svg";
  import { userInfo, isEmpty, encryptedUserInfo } from "../../../stores/info";
  import Loading from "../../Loading.svelte";
  import Pass from "../../../api/Pass";
  import { DepartmentNames } from "../../../constants/departments";
  import Barcode from "../Barcode.svelte";

  function isDepartmentName(
    name: string
  ): name is keyof typeof DepartmentNames {
    return name in DepartmentNames;
  }

  onMount(async () => {
    if (isEmpty.get()) {
      Pass.GetEncrypted().then((data) => {
        encryptedUserInfo.set(data);
      });
    }
  });
</script>

<div class="info">
  {#if !$isEmpty}
    <p>선린인터넷고등학교 모바일 학생증</p>
    <div class="detail">
      <h1>{$userInfo.name ?? ""}&nbsp;</h1>
      <h2>
        {isDepartmentName($userInfo.division)
          ? DepartmentNames[$userInfo.division]
          : ""}&nbsp;
      </h2>
      <p>{$userInfo.email ?? ""}&nbsp;</p>
    </div>

    <p>{$userInfo.date ?? ""}&nbsp;</p>
    <img src={Background.src} alt="pass" width={200} height={242} />
  {:else}
    <div class="loading">
      <Loading />
    </div>
  {/if}
</div>
<Barcode />

<style lang="scss">
  .loading {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .info {
    background: #1c232e;
    border-radius: 12px 12px 0 0;
    padding: 20px;
    height: 100%;
    min-height: 201px;
    max-height: 311px;

    display: flex;
    flex-direction: column;
    justify-content: space-between;
    position: relative;
    z-index: 0;

    img {
      position: absolute;
      right: 0;
    }

    p {
      font-weight: 400;
      line-height: 150%;
      color: var(--Blue-ish);
      font-size: 15px;
      z-index: 1;
    }

    h1 {
      color: #fff;
      font-size: 33px;
      font-weight: 600;
      line-height: 150%;
      z-index: 1;
    }

    h2 {
      color: #d7e4f5;
      font-size: 17px;
      font-weight: 600;
      line-height: 150%;
      z-index: 1;
    }

    .detail {
      display: flex;
      gap: 4px;
      flex-direction: column;
    }
  }
</style>
