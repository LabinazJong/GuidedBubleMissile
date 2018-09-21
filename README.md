# 유도미사일 발사 로직부분

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

public class BubbleShoot : MonoBehaviour {


    public GameObject buble;
    public Transform spawnPosition; 
    public Transform target; 
    public bool isguided = false;
    public float buble_delay = 0;

    void Update () {

        buble_delay += Time.deltaTime;

        isguided = CData.lockontarget;

        if (CData.fire)
        {
            if (isguided)
            {
                if (buble_delay > 0.5f)
                {
                    if (Input.GetKeyDown(KeyCode.X) && CConfig.m_fGoSpeed >= 1.0f || RealWaveJoystickInput.instance.GetKeyDown(JoyKeyCode.LRectangle) && CConfig.m_fGoSpeed >= 1.0f || RealWaveJoystickInput.instance.GetKeyDown(JoyKeyCode.RRectangle) && CConfig.m_fGoSpeed >= 1.0f)
                    {
                        GuidedBubleMissile(spawnPosition.position, spawnPosition.rotation.eulerAngles);
                        CSound.inst.effect.PlayOneShot(CSound.inst.m_Shoot);
                        buble_delay = 0f;
                    }
                }
            }

            if (!isguided)
            {
                if (buble_delay > 0.5f)
                {
                    if (Input.GetKeyDown(KeyCode.X) && CConfig.m_fGoSpeed >= 1.0f || RealWaveJoystickInput.instance.GetKeyDown(JoyKeyCode.LRectangle) && CConfig.m_fGoSpeed >= 1.0f || RealWaveJoystickInput.instance.GetKeyDown(JoyKeyCode.RRectangle) && CConfig.m_fGoSpeed >= 1.0f)
                    {
                        BubleMissile(spawnPosition.position, spawnPosition.rotation.eulerAngles);
                        CSound.inst.effect.PlayOneShot(CSound.inst.m_Shoot);
                        spawnPosition.transform.rotation = new Quaternion(0, 0, 0, 0);
                        buble_delay = 0f;
                    }
                }
            }
        }

    }

    public void GuidedBubleMissile(Vector3 position, Vector3 rotation) // 유도 버블
    {
        Vector3 newpos = target.position - position;
        newpos.Normalize();
        Quaternion newrot = Quaternion.LookRotation(newpos);

        GameObject bubleshot = (GameObject)Instantiate(buble, position, newrot);
    }

    public void BubleMissile(Vector3 position, Vector3 rotation)
    {
        GameObject bubleshot = (GameObject)Instantiate(buble, position, Quaternion.Euler(rotation));
    }
}
